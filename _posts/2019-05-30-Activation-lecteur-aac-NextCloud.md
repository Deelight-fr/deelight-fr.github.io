---
layout: post
title: Activation de la lecture des fichiers AAC dans Nextcloud
tags: [Linux, Nextcloud]
---

Une application Nextcloud permet la lecture de fichiers audio directement dans le navigateur :
[https://apps.nextcloud.com/apps/audioplayer](https://apps.nextcloud.com/apps/audioplayer)

Par défaut, elle ne permet pas de lire des fichiers AAC.

Crée un fichier `config/mimetypemapping.json` contenant :

```json
{
        "aac": ["audio/mp4"]
}
```

Lancer ensuite (dans le répertoire NextCloud) :

```bash
sudo -u www-data php occ maintenance:mimetype:update-db --repair-filecache
sudo -u www-data php occ maintenance:mimetype:update-js
```

La lecture est à présent possible.

![Challenge](/images/aac-play.png "AAC player")
