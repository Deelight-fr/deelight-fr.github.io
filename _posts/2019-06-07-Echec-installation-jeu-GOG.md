---
layout: post
title: Echec d'installation d'un jeu Linux GOG
tags: [Linux]
---

L'installation d'un jeu Linux via un installer téléchargé sur
[GOG](https://www.gog.com/) peut parfois donner lieu à une erreur :

> Erreur fatale: la création du fichier a échoué

Cela vient du fait que les installers de GOG Linux utilisent par défaut le
dossier /tmp pour décompresser les fichiers du jeu, même si l'espace dans ce
dossier est insuffisant.

Le contournement consiste a indiquer un dossier temporaire alternatif :

```bash
export TMPDIR="/autre/dossier/tmp";bash ./installer-gog.sh
```

Pour les curieux, je m'amuse en ce moment avec la version Linux de
[Full Throttle Resmastered](https://www.gog.com/game/full_throttle_remastered)
