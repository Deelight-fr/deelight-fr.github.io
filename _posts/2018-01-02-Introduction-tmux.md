---
layout: post
title: Introduction à tmux
tags: [Linux, Tmux]
---

Tmux est un multiplexeur de terminaux ; il permet d'afficher plusieurs terminaux virtuels
dans un seul terminal. Tmux peut par ailleurs être détaché d'une session pour fonctionner
en arrière-plan (à la manière de `screen`).

## Quelques commandes à connaître


| **Commande**       | **Action** |
| ------------------ | ----- |
|  `tmux`            | lancer tmux |
| CTRL-b d           | se **d**étacher de tmux |
| `tmux -a`          | se r**a**ttacher un tmux détaché |
| CTRL-b "           | diviser un terminal horizontalement  |
| CTRL-b %           | diviser un terminal verticalement  |
| CTRL-b *fleche*    | changer de fenêtre (dans la direction indiquée)  |
| CTRL-b c           | **c**réér un nouvel onglet |
| CTRL-b p           | aller à l'onglet précédent (***p**revious*) |
| CTRL-b n           | aller à l'onglet suivant (***n**ext*) |
| CTRL-b '           | aller à un onglet en fonction de son numéro |
| CTRL-b ,           | nommer un onglet |
| CTRL-b x           | fermer une fenêtre |
| CTRL-b z           | **Z**oom sur le terminal actif  (et en sortir)
| CTRL-b *espace*    | changer la disposition des fenêtres (essayer plusieurs fois de suite) |
| CTRL-b *page-haut* | remonter dans l'historique de la fenêtre |
| CTRL-b *page-bas*  | descendre dans l'historique de la fenêtre |
| *Echap*            | sortir du défilement de l'historique |
{:.mbtablestyle}

## Astuce perso

Je rajoute dans mon fichier *~/.tmux.conf* la configuration suivante :

```bash
unbind s
bind s set -g synchronize-panes
```

Ainsi, il me suffit de taper `CTRL-b s` pour activer la saisie simultanée dans toutes les fenêtres affichées.

On peut ensuite la désactiver en retapant `CTRL-b s`

## Démo en vidéo

<iframe width="560" height="315" src="https://www.youtube.com/embed/ZE1CxElW2xE" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>
(vidéo réalisée par Candyapplebone)
