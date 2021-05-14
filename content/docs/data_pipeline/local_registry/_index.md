---
weight: 3
title: "Local registry"
---

# Local registry


The documentation for the registry is available [here](https://data.scrc.uk/docs/), and is the same for the local and remote registry.

## Installation

To initialise a local registry, run the following command from your terminal:

```
/bin/bash -c "$(curl -fsSL https://data.scrc.uk/static/localregistry.sh)"
```

This will install the registry and all the related files will be stored in `~/.scrc`.

To run the server, run the `~/.scrc/scripts/run_scrc_server` script, then navigate to http://localhost:8000 in your browser to check that the server is up and running.

To stop the server, run the `~/.scrc/scripts/stop_scrc_server` script.

## Logging in

<span style="font-size:14pt; color:red">This will change...</span>

Go to http://localhost:8000/admin in your browser. Login with username admin and password admin. You can now click on View site to return to http://localhost:8000/.

After logging in you can go to http://localhost:8000/get-token to obtain an API access token.
