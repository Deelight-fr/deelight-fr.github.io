---
layout: post
title: Zimbra / Supprimer un store d'une infrastructure Zimbra
tags: [Zimbra, Linux]
---

La méthode la plus souple pour mettre à jour une infra Zimbra consiste généralement à installer de nouveaux stores sous la nouvelle version, et à migrer progressivement les boîtes mail vers ces nouveau stores. Dans mon cas d'usage, cette méthode à permis de migrer un peu plus de 12000 comptes de stores en version 8.6.0 GA vers la version 8.7.11 GA, en période de production, sans qu'aucun utilisateur ne s'en rende compte.

Comment procéder une fois que les stores d'origine sont vides ?

Dans cet exemple, nous allons supprimer un store nommé *zstoreX*, sur le domaine **domaine.tld**.

## Préalable 

- S'assurer que le store n'est pas dans le "pool de serveurs" d'une des COS existantes.
- Vérifier qu'il n'y a plus aucun compte sur le store via une recherche : ''(zimbraMailHost=zstoreX.domaine.tld)''
- Vérifier qu'il n'y a plus de sauvegardes utiles sur le store concerné (on a donc laissé le store vide actif le temps que toutes les sessions de sauvegarde en rétention soient vides).

## Suppression des références à zstoreX

On va vérifier, sur chaque serveur de l'infra, s'il y a des reférences à zstoreX dans les configurations locales :

Config locale
```bash
zmlocalconfig | grep zstoreX
```

Config serveur
```bash
zmprov gs $(zmhostname) | grep zstoreX
```

On regarde ensuite dans la configuration du domaine :

Config domaine
```bash
zmprov gd domaine.tld | grep zstoreX
```

Ensuite on regarde dans la config globale :

```bash
zmprov gacf | grep zstoreX
```

On découvre normalement que les attributs concernés sont :
- zimbraReverseProxyAvailableLookupTargets
- zimbraReverseProxyUpstreamLoginServers

Celà va evidemment varier selon l'infra.

Notons que *zmprov gs* remonte les attributs serveurs et ceux hérité de la config globale, mais on va quand meme s'assurer de supprimer une éventuelle config locale de ces attributs, sur toutes les machines de l'infra :

```bash
zmprov ms `zmhostname` -zimbraReverseProxyAvailableLookupTargets zstoreX.domaine.tld
zmprov ms `zmhostname` -zimbraReverseProxyUpstreamLoginServers zstoreX.domaine.tld
```

Ensuite on modifie la config globale, uniquement sur un serveur :

```bash
zmprov mcf -zimbraReverseProxyUpstreamLoginServers zstoreX.domaine.tld
zmprov mcf -zimbraReverseProxyAvailableLookupTargets zstoreX.domaine.tld
```

La modif est visible immédiatement sur le serveur où la commande a été lancée. On vérifie qu'il n'y a plus rien :

```bash
zmprov gacf | grep zstoreX; zmprov gs $(zmhostname) | grep zstoreX
```

Pour les autres, on doit d'abord flusher le cache de la config (la doc recommande de la faire serveur par serveur, successivement) :

```bash
zmprov flushCache config; zmprov gacf | grep zstoreX; zmprov gs $(zmhostname) | grep zstoreX
```

## Désactivation des services du store à supprimer

Sur *zstoreX*, récupérer la liste des services activés :

```bash
zmprov gs `zmhostname` zimbraServiceEnabled
```

et les désactiver (adapter cette liste au retour de la commande précédente) :

```bash
zmprov ms `zmhostname` -zimbraServiceEnabled service
zmprov ms `zmhostname` -zimbraServiceEnabled zimbra
zmprov ms `zmhostname` -zimbraServiceEnabled zimbraAdmin
zmprov ms `zmhostname` -zimbraServiceEnabled zimlet
zmprov ms `zmhostname` -zimbraServiceEnabled mailbox
zmprov ms `zmhostname` -zimbraServiceEnabled convertd
zmprov ms `zmhostname` -zimbraServiceEnabled stats
zmprov ms `zmhostname` -zimbraServiceEnabled spell
```

## Suppression du store

A lancer sur un serveur de l'infra :

```bash
zmprov deleteServer zstoreX.domaine.tld
```

La commande retournera une erreur si le store n'est pas totalement vide.

Il devrait avoir disparu de tous les stores/serveurs quand on effectue un :

```bash
zmprov getAllServers
```

## Mise à jour des clés ssh

Sur tous les serveurs restants, lancer :

```bash
zmupdateauthkeys
```
