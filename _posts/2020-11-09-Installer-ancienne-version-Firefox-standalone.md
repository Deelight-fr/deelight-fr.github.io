---
layout: post
title: Installer une ancienne version de Firefox en mode standalone
tags: [Linux]
---

Il est parfois nécessaire d'avoir à sa disposition une ancienne version de Firefox
pour pouvoir administrer certains équipements dont l'interface web n'a pu être mise
à jour.

Dans mon cas, j'avais besoin d'un Firefox <= 63.0.

```bash
mkdir ~/Documents/mon-vieux-Firefox
cd ~/Documents/mon-vieux-Firefox
wget https://gist.githubusercontent.com/rubo77/b999c1bc6d10ab802536/raw/ef97fe0b919507186a969908f4393a99e518766c/download-mozilla-portable.sh
```

A ce stade, je vous invite fortement a vérifier le contenu du script `download-mozilla-portable.sh`
que vous venez de télécharger. Executer un script inconnu téléchargé sur internet
n'est pas du tout une bonne idée.

Le deuxième arguement de ce script permet de préciser la version de Firfox que l'on
souhaite récupérer.

```bash
sh download-mozilla-portable.sh firefox 63.0
```

On obtient un exécutable de firefox 63.0 sous `~/Documents/mon-vieux-Firefox/firefox-portable-63.0/firefox-portable`.

Pour éviter qu'il se mette à jour automatiquement, ma technique un peu crade
consiste à le lancer une première fois, et à le quitter quasi-immédiatement (afin
qu'il ait le temps de créer un profil utilisateur, mais pas de se mettre à jour). 

On édite ensuite le fichier `~/Documents/mon-vieux-Firefox/firefox-portable-63.0/data/prefs.js` pour y rajouter la ligne suivante :

`user_pref("app.update.auto", false);`

C'est fini !
