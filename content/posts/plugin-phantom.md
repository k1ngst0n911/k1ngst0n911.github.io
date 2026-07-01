+++
date = '2026-06-29T19:03:39-04:00'
draft = true
title = 'Plugin Phantom'
tags = ["tenable", "nessus", "plugin", "reconstruction"]
categories = ["Reconstruction"]
+++
---
```text
·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·

          ██████   ██       ██   ██   █████   ███████  ██   ██
          ██   ██  ██       ██   ██  ██          ██    ███  ██
          ██████   ██       ██   ██  ██  ███     ██    ██ █ ██
          ██       ██       ██   ██  ██   ██     ██    ██  ███
          ██       ███████   █████    █████   ███████  ██   ██

     ██████   ██   ██    ███    ██   ██  ███████   █████   ██   ██
     ██   ██  ██   ██   ██ ██   ███  ██     ██    ██   ██  ███ ███
     ██████   ███████  ██   ██  ██ █ ██     ██    ██   ██  ██ █ ██
     ██       ██   ██  ███████  ██  ███     ██    ██   ██  ██   ██
     ██       ██   ██  ██   ██  ██   ██     ██     █████   ██   ██

                 nessus-style logic without the scanner

  ·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·
                    fetch . parse . compare . haunt
                --- greetz: no scanners were harmed ---
  ·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·-·
```

Tenable Nessus has been the de facto standard when it comes to scanning an organization's infrastructure for vulnerabilities. I wanted to show that it was possible to reproduce the behavior of a Nessus plugin via PowerShell.

Nessus plugins are merely the detection logic that allows the scanner to find things. The engine manages hosts, ports, timing, and a shared knowledge base. The plugins are where the intelligence lives. Although the setup seems daunting and complicated, the idea is rather simple. Write enough code to determine if it is susceptible to a specific vulnerability based upon observable behavior.

The bulk of the plugin feed is detection without exploitation (ACT_GATHER_INFO): version checks, banner grabbing, registry and file checks on authenticated scans, config audits. They observe state and infer vulnerability based upon said state.

The categories themselves tell you if exploitation is a part of the model:

- ACT_GATHER_INFO - Version / Banner / Config inference. No interaction beyond normal protocol use.

- ACT_ATTACK - more aggressive interaction. such as brute forcing. authentication bypass checks.

- ACT_DESTRUCTIVE_ATTACK - plugins that may crash or cause harm. disabled by default.

These plugins are written using NASL (Nessus Attack Scripting Language) that most closely resembles the C programming language.

Some of these plugins are free to inspect with *.nasl (location here) and others are .nbin compiled and encrypted?

If you are deployed via Linux, the plugins will drop here: /opt/nessus/lib/nessus/plugins/

The ones with the .nasl extension are source viewable while the .nbin ones are not (compiled). The .inc are include/library files for general use of a functionality.

I wanted to show the possibility of using PowerShell to reconstruct the behavior seen from a Tenable Nessus Plugin.

[Source Code on GitHub](https://github.com/k1ngst0n911/Shell-Circuit)
