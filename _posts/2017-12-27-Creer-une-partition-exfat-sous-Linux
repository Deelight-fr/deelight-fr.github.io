---
layout: post
title: Créer une partition exFAT sous Linux
tags: [Linux]
---

Installer les outils exfat

```bash
sudo apt-get install exfat-fuse
```

Créer la partition avec fdisk, et choisir le type de partition 11 (Microsoft basic data)

```bash
fdisk /dev/sdN
```

Formater la partition et lui attribuer une étiquette (label) :

```bash
mkfs.exfat -n VAULT /dev/sdNX
```
