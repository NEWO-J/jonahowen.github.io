---
title: "Building a Clone of Microsoft Defender's 'Attack Story' Engine"
aliases: ["/blog/defender-attackstory-clone/"]
date: 2026-08-13
showtoc: true
description: "Building a clone of Microsoft Defender XDR's Attack Story feature using a custom Policy Engine, Auth Backend, Python, Kafka, Stream Processing, and Neo4j"
tags: ["clone", "systems-design", "authentication", "kafka", "python"]
cover:
  image: "/images/defenderclone.png"
  alt: "Defender XDR Graphing Engine Clone"
---
{{< figure src="/images/passwordspraydemo.png" alt="Password Spray Demo" width="70%" >}}

Microsoft Defender's "Attack Story" feature uses a graph comprised of nodes and relationships (edges) to visually represent the path of a security incident.
Microsoft uses their own proprietary graphing engine as well as AI to achieve this, but this week I built a simpler clone using NEO4J, an open-source graphing database.

## Tech Stack
- **ALL**: Docker
- **Auth Frontend**: Nginx, Javascript, HTML
- **Auth Backend**: Python, FastAPI, PostgreSQL
- **Dashboard Frontend**: Nginx, Javascript, HTML, CSS, Vis.js
- **Dashboard Backend**: Neo4j, Python, FastAPI
- **Neo4j Connector**: Kafka, Python, Neo4j
- **Policy Engine**: Kafka, Python

