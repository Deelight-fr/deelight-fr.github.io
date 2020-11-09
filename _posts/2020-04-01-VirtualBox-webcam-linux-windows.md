---
layout: post
title: VirtualBox Ubuntu - Faire reconnaître une webcam dans un Windows virtualisé
tags: [Windows, Linux, VirtualBox, Virtualisation]
---

Il est parfois utile de permettre à un système Windows virtualisé d'accéder à la
webcam du Linux hôte (Ubuntu dans notre cas). Pour participer à une visioconférence
Skype Entreprise, par exemple.

Dans un premier temps, il faut s'assurer que l'utilisateur courant sur la machine
hôte fait bien partie du groupe `vboxusers`. Si ce n'est pas le cas :

 ```bash
 adduser login_utilisateur vboxusers
 ```

Il peut-être nécessaire de redémarrer pour s'assurer que cette modification est
bien prise en compte.

Il faut ensuite installer le pack d'extension VirtualBox :

```bash
apt-get install virtualbox-ext-pack
```

On active ensuite cette extension via l'interface de VirtualBox, dans le menu
Fichier > Paramètres > Extensions. On ajoute l'extension qui se trouve sous
*/usr/share/virtualbox-ext-pack*.

Ensuite, au niveau des paramètre de notre machine virtuelle Windows, il faut
se rendre dans la section USB et activer le *Contrôleur USB 2.0 (EHCI)*. Si la
webcam est branchée sur un port USB 3.0, sélectionner *Contrôleur 3.0 (xHCI)*.
En cliquant sur le bouton +, une liste des périphériques USB branchés à la machine
hôte devrait apparaître. Il suffit de sélectionner celui correspondant à la webcam
et le tour est joué.

Si la webcam est bien détectée par Windows mais qu'aucune image n'est visible,
vérifier que la bonne version du contrôleur USB est sélectionnée.
