---
layout: post
title: Créer une partition exFAT sous Linux
tags: [Linux]
---

Le système de fichier exFAT a l'avantage de fonctionner correctement sur tous
les OS, sans trop de bricolage. Sous Linux, il sera notamment bien moins gourmand
en CPU que NTFS, et il acceptera aussi parfaitement les fichier de plus de 4 Go.

Installer les outils exfat

```bash
sudo apt-get install exfat-fuse
```

Créer la partition avec fdisk, et choisir le type de partition 11 (Microsoft basic
data)

```bash
fdisk /dev/sdN
```

Formater la partition et lui attribuer une étiquette (label), ici "NomDuDisque" :

```bash
mkfs.exfat -n NomDuDisque /dev/sdNX
```
