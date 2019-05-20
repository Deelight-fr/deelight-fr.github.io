---
layout: post
title: Erreur lors du renouvellement d'un certificat Let's Encrypt
tags: [Linux, Sécurité]
---

Erreur obtenue lors d'un `certbot renew` :

```bash
-------------------------------------------------------------------------------
Processing /etc/letsencrypt/renewal/mon-domaine.fr.conf
-------------------------------------------------------------------------------
Cert is due for renewal, auto-renewing...                       
Renewing an existing certificate                                
Performing the following challenges:                                           
Client with the currently selected authenticator does not support any combination of challenges that will satisfy the CA.
Attempting to renew cert from /etc/letsencrypt/renewal/mon-domaine.fr.fr.conf produced an unexpected error: Client with the currently selected authenticator does not support any combination of challenges that will satisfy the CA.. Skipping.
```

Résolution (certificat utilisé dans Apache) :

```bash
sudo certbot --authenticator standalone --installer apache -d mon-domaine.fr --pre-hook "systemctl stop apache2" --post-hook "systemctl start apache2"
```
