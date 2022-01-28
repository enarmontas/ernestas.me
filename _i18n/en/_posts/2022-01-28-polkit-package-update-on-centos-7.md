---
layout: post
title: "Polkit Package Update Disrupts Docker Bridge Networking on CentOS 7"
date: 2022-01-28
tags: centos polkit redhat cve vulnerability
comments: true
summary: |
  Simple polkit pacakage update disrupts Docker bridge networking,
  so Docker service restart is required to bring the networking back.
---
Warning to anyone who is preparing for `polkit` upgrade due to
[CVE-2021-4034](https://access.redhat.com/security/vulnerabilities/RHSB-2022-001) vulnerability!

Simple `polkit` package update disrupts Docker bridge networking,
so Docker service restart is required to bring the networking back.
Service restart also restarts the containers, so expect some downtime.

Today I learned it the hard way. I planned to run `yum update -y polkit` on hundreds of servers,
because I didn't see any issues when I tested on select batch of servers before.

Well, I got lucky that I decided to double check on one more server, which also had the Docker service.

I ran the update and immediately received an alert about the app being down.
After some troubleshooting we found a [registered bug](https://bugzilla.redhat.com/show_bug.cgi?id=1435791),
which was actually closed because maintainers were unable to reproduce.

Here are the system versions:
- Docker: `19.03.15`
- CentOS: `7.8.2003`
- Kernel: `3.10.0-1127.el7.x86_64`

I hope this helps!
