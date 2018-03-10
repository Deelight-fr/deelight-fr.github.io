---
layout: post
title: Bitcoin / Installation d'un noeud Lightning sur testnet - Partie 2
tags: [Bitcoin, LightningNetwork, Cryptocurrency]
---

## Création d'un fichier ~/.lnd/lnd.conf

Celà simplifiera le lancement le lnd :

```text
[Application Options]
debuglevel=info
debughtlc=true
maxpendingchannels=10
externalip=<mon_ip_externe>
peerport=9735
  
[Bitcoin]
bitcoin.active=1
bitcoin.rpcuser=mon_login
bitcoin.rpcpass=mon_password
```

On doit d'assurer que le port 9735 est bien accessible depuis internet (redirection de port à mettre en place si nécessaire).

## Lancement de lnd

```bash
lnd --bitcoin.active --bitcoin.testnet
```

## Création d'un wallet Bitcoin

```bash
lncli create
```

On doit choisir un mot de passe. Si on relance `lnd` par la suite, il faudra déverrouiller le wallet via :

```bash
lncli unlock
```

## Génération d'une adresse Bitcoin

```bash
lncli newaddress np2wkh
```

Bien noter cette adresse car elle ne sera pas affichée ultérieurement.

## Alimenter l'adresse en tBTC (testnet bitcoins)

Via ce faucet par exemple : [https://testnet.manu.backend.hamburg/faucet](https://testnet.manu.backend.hamburg/faucet)

## Connexion à un autre noeud

On peut trouver des adresses de noeuds sur [https://explorer.acinq.co](https://explorer.acinq.co)

```bash
lncli connect 03f113414ebdc6c1fb0f33c99cd5a1d09dd79e7fdf2468cf1fe1af6674361695d2@51.15.213.104:9735
```