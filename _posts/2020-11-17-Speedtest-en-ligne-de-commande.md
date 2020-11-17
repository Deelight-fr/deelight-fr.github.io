---
layout: post
title: Effectuer un speedtest en ligne de commande sous ligne
tags: [Linux]
---

Attention : cette commande exécute un script externe. Il est impératif de vérifier
son contenu avant toute exécution. Il n'est pas nécessaire de l'exécuter en root.

```bash
curl -s https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py | python3 -
```

Exemple de retour :

```
Retrieving speedtest.net configuration...
Testing from <fai> (<ip>)...
Retrieving speedtest.net server list...
Selecting best server based on ping...
Hosted by Vialis (Colmar) [321.69 km]: 27.345 ms
Testing download speed................................................................................
Download: 900.21 Mbit/s
Testing upload speed......................................................................................................
Upload: 172.34 Mbit/s
```
