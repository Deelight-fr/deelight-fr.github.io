---
layout: post
title: Mémoriser des identifiants GIT
tags: [GIT]
---

Mise en cache des identifiants de connexion a GitHub (par exemple) dans le cas d'une connexion via un couple identifiant / mot de passe (et non par clé SSH)

```bash
git config --global credential.helper cache
```

Par défaut, le mot de passe est mis en cache pendant 15 minutes. Pour augmenter ce délai à une heure :

```bash
git config --global credential.helper 'cache --timeout=3600'
```
