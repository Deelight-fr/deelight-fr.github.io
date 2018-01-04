---
layout: post
title: Bitcoin / Installation d'un noeud Lightning sur Raspberry Pi 1
tags: [Bitcoin, LightningNetwork, Cryptocurrency, Raspberry Pi]
---

Cette procédure a été réalisée sur un Raspberry Pi 1 Modele B+
tournant sous Raspian stretch lite.

Ce Raspberry est doté d'un CPU mono-coeur cadencé à 700 MHz, et de 512 Mo de RAM.

Il s'agit plus ici de s'amuser que d'obtenir un noeud Lightning fonctionnel ;
nous sommes aux limites de ce que peut supporter un machine aussi modeste.

La blockchain a été au préalable téléchargée depuis une autre machine, puis mise à
disposition du Raspberry via un partage NFS.

## Prérequis

Augmentation de la swap à 1 Go via */etc/dphys-swapfile* :

```text
CONF_SWAPSIZE=1000
```

Prise en compte du changement :

```bash
sudo /etc/init.d/dphys-swapfile restart
```

Allocation de mémoire GPU au minimum via */boot/config.txt* :

```text
gpu_mem=16
```

Installation de Go et git :
```bash
sudo apt-get install git
wget https://storage.googleapis.com/golang/go1.8.linux-armv6l.tar.gz
sudo tar -C /usr/local -xzf go1.8.linux-armv6l.tar.gz
```

Mise à jour du PATH :
```bash
echo 'export PATH=$PATH:/usr/local/go/bin' >> $HOME/.profile
export PATH=$PATH:/usr/local/go/bin
echo 'export GOPATH=~/gocode' >> $HOME/.profile
export GOPATH=~/gocode
echo 'export PATH=$PATH:$GOPATH/bin' >> $HOME/.profile
export PATH=$PATH:$GOPATH/bin
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

Ca va prendre un bon bout de temps...

## Installation de btcd

```bash
git clone https://github.com/roasbeef/btcd $GOPATH/src/github.com/roasbeef/btcd
cd $GOPATH/src/github.com/roasbeef/btcd
glide install
go install . ./cmd/...
```

Là aussi, il faudra être patient...

## Démarrage de btcd

```bash
btcd --testnet --txindex --rpcuser=mon_login --rpcpass=mon_password
```

Le démarrage va être très long. Dans mon cas, celà a pris trois heures :

```text
2018-01-03 01:54:46.270 [INF] CHAN: Loading block index.  This might take a while...
2018-01-03 05:01:28.493 [WRN] CHAN: Unknown new rules activated (bit 1)
```

## Création d'un fichier ~/.lnd/lnd.conf

Celà simplifiera le lancement le lnd :

```text
[Application Options]
debuglevel=info
debughtlc=true
maxpendingchannels=10
externalip=<mon_ip_externe> 
  
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

Il lui faudra un petit bout de temps pour réussir à se caler avec btcd.

## Création d'un wallet Bitcoin

```bash
lncli create
Input wallet password: 
Confirm wallet password:
```

On doit choisir un mot de passe.

Durant mes multiples essais, j'ai obtenu à plusieurs reprises une erreur
*out of memory* à ce moment précis. L'augmentation de la swap (détaillée
en début d'article) a corrigé le problème. Avec un Pi doté d'un Go de RAM,
il n'y a sans doute pas de problème.

Si on relance `lnd` par la suite, il faudra déverrouiller le wallet via :

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

![BTCD + LND sur Pi1](/images/pi1-btcd-lnd.png "BTCD + LND sur Pi1")