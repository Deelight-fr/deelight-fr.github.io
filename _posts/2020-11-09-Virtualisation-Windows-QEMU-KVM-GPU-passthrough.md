---
layout: post
title: Installer une VM Windows avec un GPU dédié sous QEMU/KVM
tags: [Linux, Windows, Virtualisation, Jeu vidéo]
---

## Le projet ##

La virtualisation c'est bien beau, mais les performance graphiques des VM sont
souvent très limitées, et ne permettent pas certains usages. Dans mon cas, je
suis 95% du temps sous Linux, mais il m'arrive parfois de devoir rebooter sous Windows
pour quelques tâches très précises : traitement de photos sous Lightroom, création
musicale (mon vieux Line6 TonePort KB37 n'est pas supporté sous Linux), et jeu - bien
que la situation s'est grandement améliorée de ce côté là sous Linux ces
dernières années -.

Concernant le traitement de photos et le jeu, une carte graphique est indispensable
pour obtenir des performances correctes. C'est pour cette raison que je préfèrais
rebooter sous Windows plutôt que de passer par une VM.

Une solution existe cependant : le PCI passthrough. Il permet de dédier un
périphérique PCI (dans notre cas une carte graphique) à une VM, avec des
performances quasiment intactes.

## Prérequis ##

- Deux cartes graphiques (ou une carte + celle intégrée à votre CPU), préférablement
de deux marques différentes (NVidia et AMD par exemple)
- Beaucoup d'espace disque (Windows prend vite ses aises) voire idéalement un SSD dédié
- Pas mal de RAM (32 Go dans mon cas)
- Un CPU supportant la virtualisation processeur et chipset (chez Intel, il s'agit des
technologies VT-x et VT-d)
- Deux moniteurs

## Mon matériel ##

- Carte mère Asus H97-Plus
- CPU Intel i7 4790k
- RAM 32 Go
- GPU 1 : Nvidia K620 (que je souhaite dédier à l'hôte)
- GPU 2 : AMD Radeon RX480 (que je souhaite dédier à la VM)
- SSD 256 Go dédié à la VM

## Mise en place ##

### Configuration BIOS ###

Dans mon cas, les deux options à
activer se trouvent sous :
- `Advanced > System Agent Configuration > VT-d`
- `Advanced > Intel Virtualization Technology`

### Installation de packages ###

```bash
sudo apt-get install qemu-kvm virt-manager
sudo adduser $USER kvm
```

### Activer l'IOMMU ###

IOMMU : Input-Output Memory Management Unit

Dans */etc/default/grub* :

On remplace :
```
GRUB_CMDLINE_LINUX=""
```
Par :
```
GRUB_CMDLINE_LINUX="intel_iommu=on"
```

Puis on met à jour grub et on reboot :
```bash
sudo update-grub
reboot
```

Après reboot, le script suivant doit afficher pas mal de choses :
```bash
#!/bin/bash
shopt -s nullglob
for g in /sys/kernel/iommu_groups/*; do
    echo "IOMMU Group ${g##*/}:"
    for d in $g/devices/*; do
        echo -e "\t$(lspci -nns ${d##*/})"
    done;
done;
```

Dans mon cas je répère les deux lignes suivantes, correspondant au GPU que je
souhaite dédier à ma VM (et l'interface audio associée):
```
01:00.0 VGA compatible controller [0300]: Advanced Micro Devices, Inc. [AMD/ATI] Ellesmere [Radeon RX 470/480/570/570X/580/580X/590] [1002:67df] (rev c7)
01:00.1 Audio device [0403]: Advanced Micro Devices, Inc. [AMD/ATI] Ellesmere HDMI Audio [Radeon RX 470/480 / 570/580/590] [1002:aaf0]
```

Il faut noter les deux identifiants indiqués (dans mon cas `1002:67df` et `1002:aaf0`)

### Bloquer le chargement du GPU à dédier à notre VM ###

Dans mon cas, je ne souhaite pas charger les modules correspondant à mon AMD Radeon,
je vais donc blacklister le module `amdgpu` (vous devrez peut-être blacklister le
module `radeon` si c'est celui que vous utilisez, cf. `lsmod`).

Je crée donc le fichier */etc/modprobe.d/blacklist-amdgpu.conf* contenant :
```
blacklist amdgpu
```

J'active aussi le VFIO (Virtual Function I/O) passthrough en créant le fichier
*/etc/modprobe.d/vfio.conf* contenant :
```
options vfio-pci ids=1002:67df,1002:aaf0
```

Les identifiants sont bien évidemment à adapter.

Dernière mise à jour de grub et recontruction de l'initramfs pour prendre en
compte ces changements :

```bash
sudo update-grub
sudo update-initramfs -u
reboot
```

Après reboot, cette commande devrait afficher quelques lignes :
```bash
dmesg | grep vfio-pci
```

### Création d'une VM Windows ###

J'utilise pour celà l'outil graphique *virt-manager*. Je ne rentrerai pas dans les
détails de la création, mais le plus important est d'ajouter à notre VM deux hôtes
PCI correspondant à la carte graphique (et son interface audio) que l'on souhaite
dédier à la VM.

On n'oublie pas de brancher un écran sur notre carte graphique en passthrough,
et on lance l'installation de Windows, puis des drivers graphiques de notre carte.

### Partage souris clavier entre hôte et VM ###

On installe logiciel Barrier sur l'hôte et la VM (serveur côté hôte, client côté
VM). On pourra ainsi passer la souris d'un écran à l'autre même lorsque les deux
écrans ne sont pas connecté au même OS. Barrier permet aussi d'effectuer des
copier-coller entre le OS.

TIP : la touche "scroll-lock" (ou "verrou défil.") du clavier permet de
verrouiller la souris à l'écran actuel. Celà permet par exemple de jouer
à des FPS sans que la souris ne quitte malencontreusement l'écran du jeu en
cours de partie. Dans ce contexte, il est aussi nécessaire d'activer l'option
"mouvements de souris relatif" côté serveur Barrier pour ne pas être limité dans
ses mouvements de souris (sinon on a parfois l'impression de "buter" sur
les bords de l'écran en déplaçant la souris).

## Bonus ##

Je tenais à conserver le dual-screen sous Linux, mais à pouvoir dédier un de
mes écrans à la VM Windows lorsque je le souhaitais. Il est possible de brancher
un des écrans aux deux cartes puis de changer la source d'entrée au niveau de
l'écran, mais celà peut nécessiter un peu de développement côté Linux pour qu'il
désactive un des écrans lorsque l'on passe sur notre VM Windows, et n'est pas
forcément très érgonomique en fonction de l'écran.

J'ai opté pour une solution beaucoup plus simple : un [switch HDMI](https://www.amazon.fr/gp/product/B079FLNWJY/) (ça vaut dans
les 10€). De cette manière, lorsque je switch l'entrée connectée au GPU dédié à Windows,
mon GPU Linux détecte une déconnection physique d'un l'écran et adapte automatiquement son
affichage (même chose à la reconnexion).

![Cablage](/images/gpu-passthrough-cablage.png "Cablage")

<iframe width="700" height="388" src="https://www.youtube.com/embed/hf4wNMw6EuY" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

## Quelques photos ##

![Horizon](/images/gpu-passthrough-horizon.png "Horizon")

*Horizon Zero Dawn sous Windows à gauche, Linux à droite*

![Apex](/images/gpu-passthrough-apex.png "Apex")

*Apex Legends sous Windows à gauche, Linux à droite*
