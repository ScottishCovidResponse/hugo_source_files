---
weight: 5
title: "Notes on synchronisation"
---

# Notes

These notes are just extracted from Zulip, so are more stream of consciousness than specifications, but they attempt to describe what a command line tool might do that carried out syncing between the local and remote registries, and what else we might want it to do. Given some of the functionality described here is already provided by the [download and upload scripts](https://github.com/ScottishCovidResponse/data_pipeline_api/tree/master/data_pipeline_api/registry) in the data_pipeline_api repo, it probably makes sense for this to be written in python.

There is an alternative to this command line solution, which would be for the Django interface to the local registry to provide the same (or similar) functionality. This would allow the commands to be access through the web interface, or through the REST API and (optionally) also through the native language interfaces. This would not preclude a command line option as well, because it would simply be a wrapper around the REST API.
## Distinction between modelling API and backend

Individual languages communicate with the local registry and the local data store, but communicate no further afield. They need to be able to parse the `config.yaml` file to understand the relationship between the api requests they receive and the requests they need to make of the registry, however.

All of the syncing:

- between the remote registry and the local registry
- to populate the local data store by downloading data from remote data stores
- to create remote storage locations for local data that is being pushed to the remote registry

is handled by an as-yet-unwritten command line tool, which (for the sake of convenience) I have been calling `fdp`. This will act a little bit like `git`. I'm currently imagining it will look like:

- `fdp pull config.yaml` will pull all of the registry entries relevant to this config file into the local registry, and download any files that are referred to in `read:` or `register:` blocks to the local data store.
- `fdp run config.yaml` will run the submission script referred to in the `config.yaml`
- `fdp commit <some-token> | <path/to/file>` will mark a code run or a specific file (and all of its provenance) to be synced back to the remote registry (once we've worked out how to refer to code runs in an intuitive way?)
- `fdp push` will sync back any committed changes to the remote registry and upload any associated data to the remote data store (but we still haven't determined how to specify what to store where)

Other commands that might be useful:

- `fdp status` report on overall sync status of registry
- `fdp status <some-token> | <path/to/file>` report on sync status of a specific thing (e.g. a code run) or a specific file
- `fdp update` pull the most recent registry entries (and associated data) for anything in the current registry
- `fdp gc [--files [--all]]` maybe to remove anything from the registry that is not in the provenance history of an unsynced file? Add `--files` to also delete the associated files from the local data store unless they have restricted accessibility. Add `--all` to also delete the files from the local data store even if they have restricted accessibility.
- `fdp delete [--force] <some-token>` or `fdp delete [--force] <path/to/file>` delete a specific thing (e.g. a code run) or a specific file from the registry and local data store, adding `--force` if it is unsynced.
- `fdp info <some-token> | <path/to/file>` - return metadata or registry information on a specific registry entry or file

***

The `fdp` code will handle syncing between two registries using two REST APIs, one at each end, which (I think?) is a significantly harder problem, and so is probably best only solved once though!

***

We can't see a way of easily allowing the functionality we need without having a local registry. I think the idea is that the local registry is going to be as standalone as possible. At the moment it's a single command to install it, and a second to start it up, a third to stop it. Ideally `fdp` might be able to check if it's running and start it up and stop it on its own I guess, so the only step would be to install it.

***

In principle the "local" registry could be on a separate computer and exist for multiple users (say inside a safe haven or on HPC), but it would have to have access to the same filesystem as the machine you're running on otherwise we get back into issues of remote data access again.

## Specialising config.yaml files

One thing we haven't quite resolved is the distinction between the state of the world when we do `fdp pull config.yaml`, and the state of the world when we do `fdp run config.yaml`. A few things may have happened:

- the local registry may have been garbage collected using `fdp gc`, and now:
  1. stuff is missing that is required by the code run
- the remote registry may have been updated and:
  2. we haven't checked, so the local registry is out of date
  3. we have run another `fdp pull other-config.yaml` and so some, but possibly not all, of the local registry entries may have been updated
- the remote registry may be updated *during a run* (possibly by calling `fdp pull other-config.yaml` for a different run), and now:
  4. queries made later in a run are inconsistent with queries made earlier in a run. Note this has to happen because we want to be able to write then read something just created during a run - this is one of the requirements that necessitates having a local registry

The first two of these are probably easy to handle (crash and ignore, respectively!), but the last two are more tricky. Those may (will?) have issues for the reproducibility of the code run, because there may be no way of being able to reproduce the state of the registry at the point at which it was run. I wonder whether `fdp run config.yaml` should actually generate a fully qualified `config.yaml` file specifically for that run, that lists the exact version and data product, etc. with no globbing or text replacement left for the API code to worry about?

Alternatively this could happen when we call `fdp pull config.yaml` to generate `config-2353563.yaml`, and `fdp run config-2353563.yaml` could be executed on the fully qualified version of the file...

`config-2353563.yaml` was just the name for a file generated by `fdp` from `config.yaml` which removes all calculations and lookups, so that `human/disease/SARS-CoV-2/*` becomes a whole series of entries, one for each data product, which specifies namespace, data product and version so you know exactly what the right version was at the point you ran `fdp`.

If there's a working version of the `config.yaml` that will reproducibly generate the right aliases no matter what you subsequently do to the registry, then it makes things much simpler (and because it is what the API code is actually using, we **know** that it will generate the right aliases).

***

`fdp pull config.yaml` uses the original. Maybe `fdp run config.yaml` creates the new one and starts up the job, or maybe you have a specific `fdp generate config.yaml config-working.yaml` to generate the working one? The modelling code should never require an internet connection though.

On a related note to above, the specialised `config-working.yaml` file should have all of the `run_metadata` defaults explicitly stated so that changing your config won't change the run...

Going back to the lack of internet connectivity though, generating the working yaml file doesn't require an internet connection though. It's just saying "*given the current state of the local registry, what does this* `config.yaml` *file really mean in practice if I use it?*".

## Local data storage

How about this? You setup a new local registry, go to /Users/johnsmith/datastore and run `fdp config "johnsmith"` to set this directory as your data store. This will generate an API token for the local registry and add an entry to `.scrc/.users` noting the fact that the johnsmith username / namespace is associated with /Users/johnsmith/datastore... or do something fancier to specify the local registry url, or even set passwords? Your `config.yaml` now contains a username / namespace rather than a list of defaults associated with your account.

If someone wants to share your datastore, that's fine, they run `fdp config "otherperson"` from the same working directory and their details are added to `.scrc/.users`.

`fdp config` might also generate a specific login node, recorded in `.users`?