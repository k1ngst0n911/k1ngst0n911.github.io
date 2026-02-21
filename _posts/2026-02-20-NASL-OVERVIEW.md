---
title: NASL OVERVIEW
date: 2026-02-20
categories: vulnerabilities, detections, nasl
tags: vuln, vulns, nasl, detection
image:
  path: images/mad-scientist.png
---
Nessus is one of the cybersecurity world’s favorite flashlights for looking under the bed. It’s a vulnerability scanner used everywhere, largely because it’s very good at asking systems uncomfortable questions and writing down the answers.

In this write-up, I’m breaking down the **version-based detection logic** you’ll see inside a typical Nessus plugin. Nessus plugins are written in **NASL (Nessus Attack Scripting Language)**, a language that feels like someone fused PHP and C in a lab at 2 AM and then dared you to maintain it. We’ll unpack NASL itself another time.

For now: I wrote a NASL plugin, and I also built a **PowerShell script that simulates the same detection logic** you’d expect to see in NASL. The goal is simple: help people get comfortable with the underlying thought process driving version checks, without forcing them to immediately spelunk into NASL syntax and question their life choices.

In this example, the flow goes like this:

1. We pull the target’s **currently running version** from a REST-style endpoint.
2. We determine the **latest available version** by scraping a page that lists releases.
3. We extract the newest version number and **compare it** to what the target is running.
4. We decide whether the target is behind… and if so, we point at it politely (or not).

One important note: in real NASL plugins, you generally won’t see the plugin reaching out to some external version repository or public “latest version” page at runtime. That’s not because Nessus is shy, it’s because **on-prem deployments (like Tenable Security Center)** often live in networks where outbound traffic is treated like contraband. Firewalls, proxies, egress filtering, and general corporate paranoia can all prevent those requests from ever leaving the building.

So instead, that “latest version” knowledge is typically **hard-coded into the plugin**, keeping the detection reliable even when the scanner is operating in a network that believes the internet is a myth.

[Project Source Code](https://github.com/k1ngst0n911/Shell-Circuit)
