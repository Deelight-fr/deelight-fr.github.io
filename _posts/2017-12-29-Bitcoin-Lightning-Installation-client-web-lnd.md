---
layout: post
title: Bitcoin / Lightning - Installation du client lnd web
tags: [Bitcoin, LightningNetwork, Cryptocurrency]
---


Pocédure d'installation sous Ubuntu 16.04.3 LTS.

Le client lncli-web a été développé par [Francois Mably](https://github.com/mably)

## Installation de node et npm

```bash
sudo apt-get install npm
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get install nodejs
```

## Clonage du repository

```bash
git clone https://github.com/mably/lncli-web
cd lncli-web
```

## Installation des dépendances

```bash
npm install
```

## Configuration par défaut

```bash
"./node_modules/.bin/gulp" bundle
```

## Certificats lnd

Génération de certificats lnd compatibles avec NodeJS gRPC

```bash
pushd
cd ~/.lnd
openssl ecparam -genkey -name prime256v1 -out tls.key
openssl req -new -sha256 -key tls.key -out csr.csr -subj '/CN=localhost/O=lnd'
openssl req -x509 -sha256 -days 3650 -key tls.key -in csr.csr -out tls.cert
rm csr.csr
popd
```

Copie du certificat :

```bash
cp ~/.lnd/tls.cert lnd.cert
```

## Serveur lnd

Si ce n'est pas déjà fait, lnd doit être démarré avec l'option ```--no-macaroons```

```bash
lnd --bitcoin.active --bitcoin.testnet --no-macaroons
```

## Démarrage du serveur

```bash
node server
```

L'interface web devrait être visible à l'adresse [http://localhost:8280](http://localhost:8280)



