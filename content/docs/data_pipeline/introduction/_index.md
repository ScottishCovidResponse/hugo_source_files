---
weight: 1
title: "Introduction"
---

# Introduction

## Running Models

{{<mermaid align="left">}}
graph LR;
    subgraph Local
        LR[Local Registry]
        FS[File System]
    end
    subgraph Local API
        API
        CY
    end
    subgraph Model
        M
    end

    LR-->|read_*| API[File API]
    API-->|write_*| LR
    FS-->|read_*| API
    API-->|write_*| FS

    CY[config.yml]-->API

    API-->M[Model]
    M-->API

{{< /mermaid >}}

## Getting data

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

{{< /mermaid >}}