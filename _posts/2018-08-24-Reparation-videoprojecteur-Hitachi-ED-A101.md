---
layout: post
title: Réparation d'un vidéoprojecteur Hitachi ED-A101
tags: [Reparation]
---

Le vidéoprojecteur Hitachi ED-A101 est un système datant de 2008. Il offre une portée ultra courte permettant un affichage d'une diagonale de 150 cm sur un écran situé à une quarantaine de centimètres. Il a été massivement utilisé vers la fin des années 2000, notamment dans des universités européennes.

![Hitachi ED-A101](/images/hitachi-ed-a101.jpg "Hitachi ED-A101")

Ce modèle présente une faille de conception qui a causé la panne de bon nombre d'appareils avant même la fin de vie de la lampe qu'il contient (estimée entre 3000 et 4000 heures). Celle-ci se trouve au niveau du système de motorisation du clapet d'ouverture et de fermeture du miroir.

Au niveau de ce clapet, une roue en plastique sensée entraîner l'axe métallique transversal solidaire du miroir s'use prématurément.

![Hitachi ED-A101 roue](/images/hitachi-ed-a101-gear.jpg "Hitachi ED-A101 roue")
(source photo: vendeur ebay radadragon)

![Hitachi ED-A101 roue](/images/hitachi-ed-a101-gear2.jpg "Hitachi ED-A101 roue")
(source photo: vendeur ebay radadragon)

Hitachi a partiellement reconnu le vice de conception en permettant aux personnes concernées par le problème d'obtenir un moteur de rechange pendant 5 ans après la date d'achat (la garantie initiale était de 3 ans). D'après mes recherches, le bloc moteur de remplacement était identique à celui d'origine, donc voué à tomber à nouveau en panne.

De ce fait, des milliers de vidéoprojecteurs de ce modèle (et d'autres de la marque équipés du même dispositif d'ouverture) ont été mis à la benne. Des milliers de vidéoprojecteurs vendus initialement autours de 2500 dollars jetés en raison de la défaillance d'une petite roue en plastique de moins d'1 centimètre de diamètre...

Mais en quoi la panne du mécanisme d'ouverture rend-elle le projecteur inutilisable ? Après tout, la trappe pourrait parfaitement être ouverte manuellement. C'est là que l'affaire se corse. Le vidéoprojecteur se met en erreur dès qu'il détecte un problème d'ouverture du miroir, et ce mécanisme est quant à lui extrêmement abouti. Un interrupteur de position va détecter quand le miroir est en début et en fin de course. De plus, le vidéoprojecteur va analyser la charge du moteur pour déterminer que le miroir est en fin de course ; il va détecter quand ça "force" (sans doute une des sources du problème d'usure). Difficile donc de faire croire au vidéoprojecteur que le mécanisme d'ouverture du miroir est fonctionnel en l'ouvrant à la main.

Du fait de la popularité de ce modèle lors de sa commercialisation, beaucoup de personnes se sont penchées sur le sujet. On peut ainsi trouver :
- un [modèle 3D](https://www.thingiverse.com/thing:888215/) pour imprimer une roue de rechange
- des vendeurs de roues de remplacement en aluminium [sur ebay](https://www.ebay.fr/itm/HITACHI-GP00911-GP00912-GP00913-NOT-WHOLE-Mirror-Motor-1x-One-Gear-Repair-Kit/173464212702)
- de [longues discussions](http://www.edugeek.net/forums/av-multimedia-related/83771-hitachi-ed-a100-lens-door-error-door-wont-close.html) sur le sujet

... et surtout, [un projet à base d'Arduino](http://www.edugeek.net/forums/av-multimedia-related/83771-hitachi-ed-a100-lens-door-error-door-wont-close.html) pour leurrer le vidéoprojecteur. C'est la solution que j'ai vais explorer pour plusieurs raisons :

- cela fait pas mal de temps que je voulais réaliser un projet à base d'Arduino
- faible coût : mois de 10€ de composants, port compris
- contournement définitif de la faille de conception du vidéoprojecteur (le miroir n'est plus motorisé)
- possibilité de réutiliser les composant lorsque le vidéoprojecteur sera définitivement HS

A suivre.
