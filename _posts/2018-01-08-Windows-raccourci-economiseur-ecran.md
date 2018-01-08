---
layout: post
title: Windows - Lancer l'économiseur d'écran en un clic via un raccourci
tags: [Windows]
---

Une fois n'est pas coutume, une petite astuce Windows pour pouvoir créer dans
la barre de lancement rapide un raccourci qui lance l'économiseur d'écran
(un écran noir). Un simple mouvement de souris permettra de sortir de ce mode.

Ce besoin m'a été communiqué par une orthophoniste qui souhaitait pouvoir
éteindre temporairement son écran (déporté) lorsqu'il n'était pas nécessaire, 
pour ne pas déconcentrer ses patients.

Le fichier permettant de lancer l'économiseur d'écran est :

*C:\Windows\System32\scrnsave.scr*

Petite subtilité cependant : Windows n'accepte pas que l'on place un
raccourci vers un fichier *.scr* dans la barre de lancement rapide.

On choisit donc de créer un nouveau raccourci sur le bureau, et
d'indiquer la cible :

*C:\Windows\explorer.exe "C:\Windows\System32\scrnsave.scr"*

Ce raccourci peut ensuite être copié via un glisser-déposer dans la barre
de lancement rapide (on peut aussi modifier son nom et l'icône associée).

La raccourci du bureau peut ensuite être supprimé.