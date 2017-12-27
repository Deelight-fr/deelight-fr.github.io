---
layout: post
title: VMware & Linux / Augmentation de RAM à chaud
tags: [VMware, Linux]
---

Après ajout de RAM, voici la procédure pour qu'elle soit reconnue sans redémarrer la VM.

```bash
grep offline /sys/devices/system/memory/*/state | cut -d':' -f1 > /tmp/mem
while read a; do echo online > $a; done < /tmp/mem
rm -f /tmp/mem
```
