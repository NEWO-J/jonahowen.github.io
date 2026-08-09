---
title: "Building a Concurrent Diffing Engine for IDOR/BOLA Triage"
date: 2026-05-12
showtoc: true
description: "How I built AirGap-Diffy - a local-only HTTP differential testing tool that replays endpoints accross auth sessions to surface access-control breaks without manual comparison"
tags: ["red-team", "tooling", "python", "access-control"]
cover:
  image: "/images/diffexample.png"
  alt: "AirGap-Diffy report output"
---
During my time at Synack Red Team, the scope of real-world penetration tests initially felt overwhelming to me.
Us researchers would often be assigned massive scopes of data to audit that is unlike anything seen in a lab environment such as HackTheBox.

Fellow SRT'ers suggested I pick a vulnerability class, and get good at it - I found myself doing well with Access Control / IDOR vulnerabilites and ended up landing my first critical (CVSS 9.1) bug bounty that way.

Now that I had IDOR down, I needed to automate and reduce the triage time of sifting through these massive amounts of data.

My initial intution was to build a system that would use standard hash-diffing on each page to detect differences, this doesn't work due to the dynamic nature of modern website design, anything from a change in clock on the site, to a CSRF token rotating, would result in a false positive.

## Differential Response Testing
After going through a few options, including consideration of site screen-captures + perceptual hashing (which I decided took up way too much storage overhead and compute power for little gain). I decided that Gestalt Pattern Matching would be the most optimal algorithim for this.

Gestalt Pattern Matching works by finding the longest common substring between two sequences of data, resulting in a similarity score.

GPM is much more sensitive to small changes in data than perceptual hashing because it focuses on semantic content structure rather than perceptual image likeness. Phash will still give us a very high similarity score when subtle changes happen in the website. Phash's primary function is to detect similarities, not differences - this is not what we want.

## How It Works
My script, which i've named AirGap-Diffy (Because it adheres to SRT's strict bounds for client data, including that no data may leave the VM instance we are provided) has two primary modes for two different functions.

**Diff** is the access-control testing mode. It takes a list of URLs and a set of labeled auth sessions, fires every URL against every session concurrently, and compares each response against a designated baseline session. For each URL and session pair, it compares HTTP status codes and runs difflib.SequenceMatcher on up to 8KB of the response bodies to produce a similarity ratio. JSON responses get an additional structural diff (a recursive key comparison that shows which fields are present in one response but absent in the other). The combination of status code outcome and similarity ratio maps to a severity: CRITICAL, HIGH, MEDIUM, or INFO. The results are written to a collapsible HTML report, sorted by severity, served locally on port 7771.

{{< figure src="/images/diffexample.png" alt="The AirGap-Diffy HTML report, sorted by severity" width="120%" >}}

**Scan**, which actually does use phash takes a directory of screenshots (typically output from recon tools like GoWitness or EyeWitness) and deduplicates them visually. It hashes each image using pHash (perceptual hashing), then groups images whose hashes fall within a configurable Hamming distance of each other. The output is a single self-contained HTML report showing only the unique screenshots, stripping the noise of hundreds of identical login pages or default error screens that these tools inevitably capture.


## Limitation

## Results

