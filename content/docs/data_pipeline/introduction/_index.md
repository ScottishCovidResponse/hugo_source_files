---
weight: 1
title: "Introduction"
---

# Introduction

**This is work in progress.**

The FAIR Data Pipeline (FDP) is intended to enable tracking of provenance of data used in epidemiological modelling. Pipeline APIs written in C++, Java, Julia, Python and R can be called by modelling software for data ingestion. These interact with a local relational database storing metadata and the local filesystem, and are configured using a yaml file associated with the model run. Local files and metadata can be synchronised with a remote registry via a command line tool (`fdp`).

The key benefits of using the FAIR Data Pipeline are:

- Data recorded in a FAIR fashion (metadata on all data and code open and available for inspection)
- Provenance tracing allows model outputs to be traced to inputs and modelling code
- Multiple language support
- Designed to run on a broad range of platforms (including HPC, inside Safe Havens)
- Designed to be set up and completed online (to down-/up-load data) and run offline (Safe Havens will require this)
- Open metadata provides knowledge of or access to shared central data for specific domains (e.g. COVID-19 epidemiological modelling)

## Running Models

To use the FAIR Data Pipeline with a piece of modelling software, you must add a language specific Pipeline API as a dependency and interact with data registered in the pipeline via the methods it presents. Each model run must be configured using a [`config.yml`](../interface/_index.md) file which specifies inputs and outputs by metadata.

{{<mermaid align="left">}}
graph LR;
    subgraph Local
        LR[Local Registry]
        FS[File System]
    end
    subgraph Local API
        API[Pipeline API]
        CY[config.yml]
    end
    subgraph Model
        M[Model]
    end
 
    LR -->|read_*| API
    API-->|write_*,link_*| LR
    FS-->|read_*| API
    API-->|write_*| FS

    CY-->API

    API-->|read_*,link_*|M
    M-->|write_*,link_*|API
    M-->|link_*|FS

{{< /mermaid >}}

## Getting data

The command line utility `fdp` is used to download and upload data and metadata required for and produced by model runs.

{{<mermaid align="left">}}
graph LR;
    subgraph Remote
        RR
        OS
        URI
    end
    subgraph Local
        LR
        FS
    end

    RR[Remote Registry]-->|fdp pull| LR[Local Registry]
    LR-->|fdp push| RR

    RR-->OS(Managed Object Store)
    RR-->URI(Abribtrary URI)
    LR-->FS(Local Filesystem)

    OS-->|fdp pull| FS
    URI-->|fdp pull| FS
    FS-->|fdp push| OS
    FS-->|fdp push?| URI

{{< /mermaid >}}
