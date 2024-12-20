---
layout: post
title: Recyclage d'un NAS Ve-Hotech VHS4 LS
tags: [Linux, Reparation]
---

![VHS4](/images/vhs4.png "VHS4")

Le NAS Ve-Hotech VHS4 LS est un NAS 4 baies. Il tourne sous une version modifiée d'Ubuntu 10.04 (obsolète depuis le 9 mai 2013), et n'offre qu'un accès limité au système à l'utilisateur (gestion via une interface web, pas d'accès SSH).

La société [Ve-Hotech](https://fr.wikipedia.org/wiki/Ve-hotech) a été mise en liquidation judiciaire le 04/09/2019, ne proposant de fait plus aucune mise à jour.

Du fait de l'obsolescence de leur système on les trouve à très bas prix sur les sites de petites annonces, malgré leur hardware encore tout à fait adapté à un NAS. Mon objectif était donc de parvenir à installer une distribution Linux récente.

Probléme : le VHS4 a toute l'apparence d'une boîte noire car il n'est pas (de base) possible d'y brancher un écran pour accéder au BIOS et tenter de démarrer sur un autre disque. D'ailleurs, parlons du disque système d'origine du VHS4. Une inspection rapide de la carte mère ne permet pas de l'identifier. Pas de carte SD ou périphérique interne, pas de DOM (Disk on Module)... Une énigme.

On trouve cependant sur la carte mère un connecteur 14 broches (13 en réalité, une est absente) nommé JVGA.

![VHS4-Board](/images/vhs4-board.png "VHS4-Board")

En recherchant les références de cette dernière (MSI MS-S0031), j'ai trouvé le [mode d'emploi](https://www.manualsdir.com/manuals/467282/msi-ms-s0031.html) correspondant, indiquant le brochage de ce connecteur.

![VHS4-JVGA](/images/vhs4-jvga.png "VHS4-JVGA")

Me voilà donc parti pour tenter de réaliser un adaptateur maison.

![VHS4-VGA](/images/vhs4-vga.png "VHS4-JVGA")

* JVGA1 pin 1 -> VGA pin 1
* JVGA1 pin 3 -> VGA pin 2
* JVGA1 pin 5 -> VGA pin 3
* JVGA1 pin 11 -> VGA pin 13
* JVGA1 pin 12 -> VGA pin 14

La connexion de tous les pins Ground (2, 4, 6, 7 et 9 côté JVGA1 et  5, 6, 7, 8, 10) permet d'avoir une image avec moins d'interférences. Les autres connexions sont optionnelles.

Heureusement j'avais pas mal de composants qui trainaient, notamment un PCB disposant d'un port VGA male. J'ai aussi pu utiliser un vieux connecteur IDE 44 broches male (ceux qui se branchent sur les anciens disques IDE 2'5) qui, une fois coupé, s'avère s'insérer parfaitement sur le connecteur JVGA1 de la carte mère. 

![VHS4-VGA-Connector](/images/vhs4-jvga-connector.png "VHS4-JVGA-Connector")

On peut sans doute trouver ce type de connecteur dans [toute bonne boutique d'électronique](https://fr.farnell.com/hirose-hrs/a3-14da-2sv-71/connecteur-rcpt-14-voies-2-rangs/dp/2724831) (rechercher embase, 14 voies, 2 rangs, pas de 2mm...).

Restait à faire les soudures, essayer de ne pas se planter dans les correspondances... et trouver un moniteur disposant d'un port VGA.

Au bout de plusieurs heures à essayer de trouver les bonnes correspondances, à souder, désouder, tester... une image apparaît enfin à l'écran. Les couleurs sont mauvaises à cause de mon vieil écran qui est en train de rendre l'âme, mon montage est atroce... mais peu importe, c'est utilisable et je peux accéder au BIOS en tapant sur la touche Del au démarrage.

![VHS4-Ugly](/images/vhs4-ugly.jpg "VHS4-Ugly")
![VHS4-BIOS](/images/vhs4-bios.jpg "VHS4-BIOS")

Je découvre ainsi qu'il y a bien, quelque part dans le boîtier, un disque USB 4 Go hébergeant l'OS du NAS. Peu importe, je modifie l'ordre de priorité des disques de demarrage et décide de démarrer sur une clé USB branchée sur un des ports à l'arrière de boîtier. Bonne surprise, celà fonctionne parfaitement sur la dernière Debian stable en date (12.4). A des fins de recherche, j'en profite pour faire un dump du disque système d'origine pour l'étudier utltérieurement.

J'installe donc l'ISO d'installation de la dernière version d'OpenMediaVault sur ma clé, je fouille dans mes tiroirs pour trouver une autre clé USB, la plus petite possible (en dimensions), qui va devenir de nouveau disque système du NAS, branchée en permanence à l'arrière du boîtier. Je préfère ne pas altérer la partition de 4 Go d'origine du NAS, mais c'est un choix personnel, pour pouvoir éventuellement remettre le NAS dans son état (obsolète) d'origine.

![VHS4-Screenshot](/images/vhs4-screenshot-new.png "VHS4-Screenshot")

A suivre...
