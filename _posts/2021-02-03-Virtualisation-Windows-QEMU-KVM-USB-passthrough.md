---
layout: post
title: Installer une VM Windows avec une carte USB dédiée sous QEMU/KVM
tags: [Linux, Windows, Virtualisation]
---

Suite à mon [article précédent](http://blog.deelight.org/Virtualisation-Windows-QEMU-KVM-GPU-passthrough)
concernant la virtualisation KVM/QEMU avec GPU en passtrough, j'ai utilisé pendant plusieurs mois cette
solution sans problème particulier.

Un limitation demeurait cependant pour quelques utilisations très spécifique, et notamment la MAO (musique
assistée par ordinateur). Je dispose en effet d'une carte son USB externe qui, une fois partagée avec une VM,
induisait des latences la rendant inutilisable.

Il était donc nécessaire de procéder de la même façon qu'avec la carte graphique, en dédiant un controlleur USB
à ma VM. J'ai donc acheté une carte PCI offrant 5 ports USB 3.0 (Inateck KTU3FR-5O2I) pour la dédier à ma VM.

Tout ne s'est malheureusement pas passé comme prévu car en ajoutant l'identifiant de ma carte dans
*/etc/modprobe.d/vfio.conf* puis en la rattachant à ma VM via le partage d'hôte PCI, ma VM refusait tout
simplement de booter ("No boot device").

J'ai trouvé sur un forum [un témoignage similaire](https://forums.unraid.net/topic/91871-windows-vm-usb-host-controller-no-bootable-drive/).
Conformément à la réponse donnée qui indique que certaines cartes PCI peuvent poser problème en passthrough, j'ai
changé de méthode et décidé de dédier un contrôleur USB de ma carte mère à ma VM, et d'utiliser ma carte USB 3.0 PCI
pour l'hôte. Et là, plus aucun problème.

Ma carte son externe fonctionne désormais à merveille dans ma VM, sans aucune latence. Je dispose par ailleurs
ne nombreux autres ports USB directement reliés à ma VM si le besoin s'en faisait sentir.
