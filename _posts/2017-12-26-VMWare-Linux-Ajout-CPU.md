---
layout: post
title: VMware & Linux / Ajout de processeurs à chaud
tags: [VMware, Linux, Virtualisation]
---

Après ajout d'un ou plusieurs CPU, voici la procédure pour qu'ils soient reconnus sans redémarrer la VM.

```bash
cd /sys/devices/system/cpu/
for F in cpu* ; do echo 1 > "$F/online"; done
```
