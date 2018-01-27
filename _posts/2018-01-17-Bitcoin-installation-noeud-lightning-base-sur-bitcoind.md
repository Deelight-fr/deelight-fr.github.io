---
layout: post
title: Bitcoin / Installation d'un noeud Lightning testnet bitcoind/lnd
tags: [Bitcoin, LightningNetwork, Cryptocurrency]
---

Installation d'un noeud lightning bitcoind (au lieu de btcd) + lnd sous Ubuntu 16.04.3 server

Pour n'oublier aucune étape, je suis parti d'une machine virtuelle avec une installation vierge et à jour de la distribution. Rien d'autre n'a été installé hormis les groupes logiciels :

- standard system utilities
- OpenSSH server

## Bitcoind

### Installation

```bash
sudo apt-add-repository ppa:bitcoin/bitcoin
# appyer sur entrée pour accepter l'avertissement
sudo apt-get update
sudo apt-get install bitcoind
```

### Configuration

```bash
mkdir ~/.bitcoin
```

Création du fichier *~/.bitcoin/bitcoin.conf* :

```text
testnet=1
txindex=1
server=1
daemon=1
zmqpubrawblock=tcp://127.0.0.1:18501
zmqpubrawtx=tcp://127.0.0.1:18501
rpcuser=mon_login
rpcpassword=mon_password
```

### Lancement et synchronisation de la blockchain

On peut désormais lancer bitcoind et attendre que la chaîne se synchronise.

```bash
bitcoind
```

On peut suivra l'avancement de la synchro via :

```bash
tail -f ~/.bitcoin/testnet3/debug.log
```

On peut aussi récupérer la blockchain d'un autre bitcoind déjà synchronisé. Il s'agit des répertoires suivants :

- *~/.bitcoin/testnet3/chainstate/*
- *~/.bitcoin/testnet3/blocks/*

## Lnd

### Prérequis

Installation de Go et git :

```bash
sudo apt-get install golang-1.9 git
```

Mise à jour du PATH :

```bash
echo 'export GOPATH=~/gocode' >> $HOME/.profile
export GOPATH=~/gocode
echo 'export PATH=$PATH:$GOPATH/bin:/usr/lib/go-1.9/bin' >> $HOME/.profile
export PATH=$PATH:$GOPATH/bin:/usr/lib/go-1.9/bin
```

Installation de Glide :

```bash
go get -u github.com/Masterminds/glide
```

### Installation

```bash
git clone https://github.com/lightningnetwork/lnd $GOPATH/src/github.com/lightningnetwork/lnd
cd $GOPATH/src/github.com/lightningnetwork/lnd
glide install
go install . ./cmd/...
```

### Configuration

```bash
mkdir ~/.lnd
```

Création d’un fichier *~/.lnd/lnd.conf* :

```text
[Application Options]
debuglevel=info
debughtlc=true
maxpendingchannels=10
externalip=mon_ip_externe:9735
listen=localhost:9735
alias=mon node lightning

[Bitcoin]
bitcoin.active=1
bitcoin.node=bitcoind
bitcoin.testnet=1
```

Pas besoin de préciser les paramètres de connexion à bitcoind, lnd va automatiquement les récupérer depuis `~/.bitcoin/bitcoin.conf`.

On doit d’assurer que le port 9735 est bien accessible depuis internet (redirection de port à mettre en place si nécessaire).

### Lancement

```bash
lnd
```

#### Création d’un wallet Bitcoin (on doit choisir un mot de passe) :

```bash
lncli create
```

Si on relance lnd par la suite, il faudra déverrouiller le wallet via :

```bash
lncli unlock
```

#### Génération d’une adresse Bitcoin

```bash
lncli newaddress np2wkh
```

Bien noter cette adresse car elle ne sera pas affichée ultérieurement.

#### Alimenter l’adresse en tBTC (testnet bitcoins)

Via ce faucet par exemple : [https://testnet.manu.backend.hamburg/faucet](https://testnet.manu.backend.hamburg/faucet)

#### Connexion à un autre noeud

On peut trouver des adresses de noeuds sur [https://explorer.acinq.co](https://explorer.acinq.co)

```bash
lncli connect 03f113414ebdc6c1fb0f33c99cd5a1d09dd79e7fdf2468cf1fe1af6674361695d2@51.15.213.104:9735
```

### Création d'un channel

On alloue dans cet exemple 300000 satoshis au nouveau channel.

```bash
lncli openchannel 02464bfaaae78b98268a6a6d7e8f6a110c60dd1293811d6029b11ee9edb4bbf869 --local_amt 300000
```

A partir d'ici, on peut effectuer des paiements, se connecter à d'autres noeuds, ouvrir de nouveaux channels etc.

## Ressources

- [Testing lnd with a bitcoind back-end](https://gist.github.com/aakselrod/5644b9319041a796ba6ffca28062376e)
- [Lightning Network Developers: Installation](http://dev.lightning.community/guides/installation/)
- [Lightning Network Developers: Stage 1 - Setting up a local cluster](http://dev.lightning.community/tutorial/01-lncli/index.html)
- [Lightning Network 2018 - Acinq.fr - Fabrice Drouin](https://youtu.be/DxqTzTwFKnM)
