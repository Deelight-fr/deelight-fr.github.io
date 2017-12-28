---
layout: post
title: GitHub / Authentification par clé SSH
tags: [Git, GitHub, Linux]
---

Copier la clé publique sur la page [SSH Keys](https://github.com/settings/keys) de GitHub.

On peut ensuite tester la connexion via :

```bash
ssh -T git@github.com
```

Le serveur doit répondre :

```
Hi <username>! You've successfully authenticated, but GitHub does not provide shell access.
```

Dans le répertoire d'un dépôt local, il suffit de configurer :

```bash
git remote set-url origin git@github.com:username/depot.git
```

Les prochains ```git push``` utiliseront l'authentification par clé SSH.