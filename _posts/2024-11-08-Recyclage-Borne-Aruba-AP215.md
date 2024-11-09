---
layout: post
title: Recyclage d'une borne Wifi Aruba AP-215
tags: [Linux, Reparation]
---

![Aruba AP215](/images/aruba-ap215.jpg "Aruba AP215")

Les bornes d'accès sans fil HPE Aruba AP-215 sont par défaut gérées par un contrôleur dédié. Il est cependant possible de les passer en mode "Aruba Instant" afin de pouvoir se passer de contrôleur.

Pour passer dans ce mode, on va devoir flasher l'OS de la borne.

## Prérequis

- Pouvoir alimenter la borne AP-215, soit via un connexion POE si votre switch le supporte, soit via un bloc d'alimentation dédié (12V / 1,5A).
- Un câble de console USB -> RJ45 (RS232), ou USB -> DB9 -> RJ45
- Récupérer la dernière version d'Aruba Instant AOS compatible avec la borne ([8.6.0.25](https://networkingsupport.hpe.com/downloads/software/RmlsZTpmNDQwOThmYS00ZjY1LTExZWYtOTdlNC00MzQ0NTRhNTcxNDQ%3D)). Nécessite un compte support.

## Mise en place d'un serveur TFTP

On crée un serveur TFTP sur notre réseau (par exemple tftpd sous Linux ou avec Open TFTP Server sous Windows), et on place à la racine le fichier ArubaInstant_Centaurus_8.6.0.25_90367.

## Flashage de la borne

![Aruba AP215 Back](/images/aruba-ap215-back.jpg "Aruba AP215 Back")

On connecte tout d'abord le câble console au port "CONSOLE" de la borne et on ouvre une connexion série. On peut ensuite connecter la borne au réseau (et l'alimenter).

On doit voir défiler les logs de démarrage sur la connexion série.

Lorsque la borne affiche `Hit <Enter> to stop autoboot:`, taper sur la touche Entrée pour obtenir un prompt (`apboot>`).

On indique à la borne de récupérer une IP via DHCP et on note son adresse (ici 192.168.0.20).

```bash
apboot> dhcp
...
DHCP IP address: 192.168.0.20
...
```

On indique l'adresse IP de la machine sur laquelle est lancé le service TFTP (ici 192.168.0.10).

```bash
apboot> setenv serverip 192.168.0.10
```

On peut ensuite lancer la mise à jour de l'OS.

```bash
apboot> upgrade os ArubaInstant_Centaurus_8.6.0.25_90367
eth0: link up, speed 100 Mb/s, full duplex
Using eth0 device
TFTP from server 192.168.0.10; our IP address is 192.168.0.20
Filename 'ArubaInstant_Centaurus_8.6.0.25_90367'.
...
Copying to flash...
Writing 9....8....7....6....5....4....3....2....1....done
Verifying flash... 14603004 bytes were the same
Upgrade successful.
```

On identifie le numéro de la partition de boot (normalement 0).

```bash
apboot> osinfo
Partition 0:
...
```

On force le boot sur cette partition.

```bash
apboot> setenv os_partition 0
Partition 0:
...
```

On peut ensuite sauvegarder la configuration et rebooter la borne.

```bash
apboot> save
apboot> reset
```

## Paramétrage de la borne via l'interface web

Se connecter à https://192.168.0.20 (l'IP de votre borne ; il faudra sans doute accepter le certificat auto-signé).

Les identifiants de connexion par défaut sont :
* identifiant : admin
* mot de passe : numéro de série de la borne

L'interface vous proposera immédiatement de choisir un nouveau mot de passe, et vous aurez accès à la configuration de votre borne.

![Aruba AP215 web](/images/aruba-ap215-web.png "Aruba AP215 web")

## Références

* [Reddit - AP Convert command in 8.6 lets you convert Campus APs (215, 225, 315, etc..) to IAP](https://www.reddit.com/r/ArubaNetworks/comments/grunb4/comment/g6p7z2j/)
* [acmxguy - Aruba Instant – AP boot image upgrade](https://acmxguy.wordpress.com/2020/05/06/aruba-iap-ap-boot-image-upgrade/)
* [ArubaNetworks - Guide d’installation (PDF)](https://www.arubanetworks.com/techdocs/hardware/aps/ap210/ig/AP210%20Series%20IG%20Rev%2001_FR.pdf)
