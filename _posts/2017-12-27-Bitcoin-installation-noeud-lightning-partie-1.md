---
layout: post
title: Bitcoin / Installation d'un noeud Lightning sur testnet - Partie 1
tags: [Bitcoin, LightningNetwork, Cryptocurrency]
---

Cette procédure a été effectuée sur un Linux Ubuntu Server 16.04.3 LTS. Prévoir assez d'espace disque pour héberger un full node Bitcoin testnet (le 27/12/2017, la blockchain prenait 15,3 Go).
Il s'agit globalement d'une traduction très épurée de [cette documentation](http://dev.lightning.community/guides/installation).

## Prérequis

Installation de Go :
```bash
apt-get install golang-1.9
```

Mise à jour du PATH (potentiellement à rajouter dans ~/.bashrc):
```bash
export GOPATH=~/gocode
export PATH=$PATH:$GOPATH/bin:/usr/lib/go-1.9/bin/
```

Installation de Glide :
```bash
go get -u github.com/Masterminds/glide
```

## Installation de lnd

```bash
git clone https://github.com/lightningnetwork/lnd $GOPATH/src/github.com/lightningnetwork/lnd
cd $GOPATH/src/github.com/lightningnetwork/lnd
glide install
go install . ./cmd/...
```

Par la suite, il sera possible de mettre à jour lnd via :

```bash
cd $GOPATH/src/github.com/lightningnetwork/lnd
git pull && glide install
go install . ./cmd/...
```

## Installation de btcd

```bash
git clone https://github.com/roasbeef/btcd $GOPATH/src/github.com/roasbeef/btcd
cd $GOPATH/src/github.com/roasbeef/btcd
glide install
go install . ./cmd/...
```

## Démarrage de btcd

```bash
btcd --testnet --txindex --rpcuser=mon_login --rpcpass=mon_password
```

La synchronisation de la blockchain va débuter. Ce processus peut prendre plusieurs heures.

On peut obtenir le numéro du dernier block synchronisé via :

```bash
btcctl --testnet --rpcuser=mon_login --rpcpass=mon_password getinfo | grep blocks
```
