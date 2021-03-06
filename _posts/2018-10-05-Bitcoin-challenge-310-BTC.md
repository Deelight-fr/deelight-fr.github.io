---
layout: post
title: Bitcoin Challenge / 310 BTC cachés dans une image
tags: [Bitcoin]
---

Dans l'esprit du challenge du [challenge du logo de l'ANSSI](https://www.ssi.gouv.fr/actualite/le-challenge-anssi-qui-avait-ete-lance-en-fevrier-2012-a-ete-resolu/), un anonyme a lancé le 2 octobre sur [Reddit](https://www.reddit.com/r/Bitcoin/comments/9kq7it/introducing_the_310_btc_bitcoin_challenge) et [BitcoinTalk](https://bitcointalk.org/index.php?topic=5042285) une invitation à relever un challenge : trouver la clé privée [cachée dans une image](https://bitcoinchallenge.codes) donnant accès à [un wallet contenant 310 BTC](https://www.blockchain.com/btc/address/39uAUwEFDi5bBbdBm5ViD8sxDBBrz7SUP4). L'auteur a déjà apporté la preuve cryptographique qu'ils étaient en sa possession en signant l'url du challenge avec la clé privée du wallet.

Je ne me fais pas de faux espoirs, il y a des gens bien plus compétents que moi qui emporteront le prix. Cependant, la résolution de l’énigme reste un défi passionnant. J'indiquerai donc sur cette page l'état de mes recherches. N'hésitez pas à partager les vôtres dans les commentaires. Si vous souhaitez que l'on planche à plusieurs, n'hésitez pas non plus ; on pourrait par exemple se retrouver sur un canal IRC dédié. Bonne chance à tous !

## L'image source

![Challenge](/images/310-bitcoin-challenge.png "Challenge")

## Les découvertes ##

### Le tableau de 18 valeurs ###

Cette énigme a été résolue dès le deuxième jour du challenge. Elle permet d'obtenir la clé privée d'un [wallet qui contenait initialement 0.1 BTC](https://www.blockchain.com/btc/address/1446C8HqMtvWtEgu1JnjwLcPESSruhzkmV).

La première étape consiste à repérer une date cachée dans l'image.

![Challenge](/images/310-bitcoin-challenge-date.png "Challenge")

"OCT 2 2018" nous donne en format propre (= pas américain) : "20181002".

Nous prenons ensuite les caractères de cette chaîne 1 par 1 (en répétant la chaîne au besoin) et les soustrayons aux caractères du tableau initial (en hexadécimal). En cas de valeur négative, on repart de la valeur maximum (F en hexadécimal) et on soustrait le reste.

Si ce n'est pas clair, imaginez un cadenas à code en hexadécimal (donc des molettes à 16 positions). si vous devez soustraire 4 d'une molette qui se trouve sur 2, vous allez passer de 2 à 1, de 1 à 0, de 0 à F, et enfin de F à E. On peut faire ce calcul en convertissant en décimal et utilisant notre cher modulo : 2 - 4 = -2 et -2 mod 16 = 14 (E en hexadécimal).
Le nombre de crans pour chaque molette correspond à ce qu'indique chaque caractère correspondant dans la clé "20181002" : descendre la première molette de 2, la deuxième de 0, la troisième de 1...

On constate ainsi que la première ligne nous donne une série de "310" qui nous suggère que nous sommes sur la bonne piste. Toutes les autres valeurs du tableau donnent des chiffres (une fois la conversion hexadécimal vers décimal effectuée) inférieurs à 2048. Ces valeurs correspondent à un codage possible en BIP-0039 (liste de mots parmi [2048 possibilités](https://github.com/bitcoin/bips/blob/master/bip-0039/english.txt)).

![Challenge](/images/310-bitcoin-challenge-table-decoding.png "Challenge")

La formule utilisée dans chaque cellule du tableau C (attention les yeux) :

```
=dec2hex(mod(hex2dec(mid(CELLULE_DE_A;1;1))-mid(CELLULE_CORRESPONDANTE_DE_B;1;1);16)) &
dec2hex(mod(hex2dec(mid(CELLULE_DE_A;2;1))-mid(CELLULE_CORRESPONDANTE_DE_B;2;1);16)) &
dec2hex(mod(hex2dec(mid(CELLULE_DE_A;3;1))-mid(CELLULE_CORRESPONDANTE_DE_B;3;1);16))
```

On obtient donc 12 mots qui correspondent à la clé privée d'un [wallet contenant 0.1 BTC](https://www.blockchain.com/btc/address/1446C8HqMtvWtEgu1JnjwLcPESSruhzkmV).

```
cry buyer grain save vault sign
lyrics rhythm music fury horror mansion
```

- Adresse : 1446C8HqMtvWtEgu1JnjwLcPESSruhzkmV
- Clé publique : 0376a376c4c2fc0ffad1a5d87f2100343d2cd29a5f7859458e545857727133e349
- Clé privée : KzkZxdhRGxB7eX4u1skXkfJ7VB8JfPp7Nfos3jiF7PQUNMh2SHDE

### Le masque de transparence ###

Le PNG d'origine présente des zones légèrement transparentes invisibles à l’œil nu (opacité de 253 sur 255). Voici comment les mettre en évidence avec Gimp :

<iframe width="560" height="315" src="https://www.youtube.com/embed/63ltk3OcTq8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

Le QRcode amène vers une [page du site](https://bitcoinchallenge.codes/register-310/) où l'on peut tenter de valider la hash sha256 d'un fichier. La ligne correspond quant à elle à du binaire :

```
0101010100110010010001100111001101100100010001110101011001101011010110000011000100111001010100010011001101001001001011110010111101010110010000110100100000110000010101010011001101100011010101100111010001001001010101000101101000110011011000110110101101001001010011000100101001101110010101010110001101100100010100000101100000110011010001110111001100110101011100010110101001100100010001100011000101010101011010100101101000110011011011010100000101100110011101000100011101101001011101100111010001000110010110010100010000001010010011100011010101011010010000110101001101101011010000100111100101101110011011100101011001110001010000100110000101110111011011000011010001110000001110000111011101001011010011110011000001001111001110000111101001001001001101100100010000110000010000010011000100101011010101100100010101010110010000110101010101111001010001010111011001000101011001010100111001101111010101010110011001000111011000110101001100110000010001010110110000111001011001000011100100110011011101100111001101010000011110000110001001100111001101110000101001000100001101010110000101110110011101010110011001010001011100110101001101100011011001110111001101101011001100110101000101000101011101000111000100111001001011110100110100110100010001000110111100110011001100100100111101001011010001100110010101110001001100000011000000101111001100110100111001110010011110000101011101001111011100110100110101101101011010000011001101000001010110000110110101000100011110100111010101110101010110100011000001110001011011010101101001100001010010010011011101110010011001010011000100110110000010100100011001100011010110000100100101110010011011010101000001010000011010010101000101000100010011110100100001010010011000110011011101110111011101000011000001101110011001110011011001110001010011000110100101001110011110100011011101010110011100010100010101010011010100100101010001100100011110000101000001001111011000010110100001001011010001100101001001101011010101110101010000111000011100110101010000101011010101010111001000110010011110010010101100110010011010010101101000110010010011000100010101100001011110000100111000001010010011010011011101010101010110100111000101100011010100000111011101011001011001110110110100110110010001100110111101001011010011110101011001101010011011100111000101100100011001010110011100110011001100000101001000110010001101110110101001100011001101100100000101101111010001100101000001111001010100100101101000110010011001110011100000101011010001010100101001001101011100000011001101101110001011110111000001100110001110010011010001101111010100110100001101001100010001010101011101101011011000110011000001101111011100110000101001101010010010000011100101000100011100010110001001001101001101100100010001010101011100000111010001110101001100110100100001001010011000100100000101010110011101110101100001010001001111010011110100001010
```

Converti en ASCII, cela donne :

```
U2FsdGVkX19Q3I//VCH0U3cVtITZ3ckILJnUcdPX3Gs5qjdF1UjZ3mAftGivtFYD
N5ZCSkBynnVqBawl4p8wKO0O8zI6D0A1+VEVCUyEvEeNoUfGcS0El9d93vsPxbg7
D5avufQsScgsk3QEtq9/M4Do32OKFeq00/3NrxWOsMmh3AXmDzuuZ0qmZaI7re16
FcXIrmPPiQDOHRc7wt0ng6qLiNz7VqESRTdxPOahKFRkWT8sT+Ur2y+2iZ2LEaxN
M7UZqcPwYgm6FoKOVjnqdeg30R27jc6AoFPyRZ2g8+EJMp3n/pf94oSCLEWkc0os
jH9DqbM6DUptu3HJbAVwXQ==
```

Si l'on fait un décodage base64, la chaîne obtenue débute par "Salted". C'est visiblement un fichier de 256 octets chiffré via OpenSSL. Reste à trouver l'algorithme de chiffrement et la clé.

On appellera ce fichier **alpha.enc**.

### Extraction du LSB du canal rouge ###

LSB : Least Significant Bit (bit de poids faible)

On comparant tout les couches visibles de l'image d'origine, on constate des différences dans la couche rouge, notamment sur la ligne 310.

En extrayant l'inverse du dernier bit de la valeur de chaque pixel de la ligne 310 du canal rouge, on obtient le code binaire suivant :

```
0100101010111001010011100111101101111011000011001110010100110000010110000011001001111101000100000110011100001000001011110010101010010111100010100010101001110100011001010011001101100011100001101100111100010110001001010111001000100110111011111100100001010011111111011001111101111100001111101001011101010010001100110001000000100000000100110011101011111000111101011100010110001011001111011110010001101110100011100101010110011100011100111111000001001100010000110001111001110101110101001101010011101110101100111010111011100011101010111111000110100110010001101111010011101000010000110111001010111100000001011011011010101100110011111101101010100110101101010110111111111111100110010000111101100001000000100001101011000111111010110110110101100110000111011101011011011001011011011010010000010101001110111010011101100111000111001101010001110111001111001111000000111000110010111101101111000011000010001101110110110111010101111000001010111001011010110001001000100110000011110111100101000010001010010111111000111010110010000010111111110010100101100011010010100000111110000010100111000100111011001110000001101000111110100001101011011111000110010100100111010000001101111100000111110000110010000100010001011010010111011011010111010110110111010111101001101110011100000111001111000000010010110111001010110011011101110111001010101011100110010000111000101000000011101001110001001110101001101000010111111000000101110011011111011010101010111011110010011001111000111100110001010110100111101000001100011100111101101010100000101101101010100111101111101000010000110010000111010111011001011011110110111111011001001011110100110111000110000100011110100000011100100001001010101101110010001100111111010100001110001111111100001101110111100011111101010000101000011001111110110000000010011011011000111100101000101101101010110100001011011111000110100011010000111111001100110100011100110011000111000100100111111001111010100001100111000001100010010100010000110000110101101011110010100101000000101010010101010111001000110010011110010010101100110010011010010101101000110010010011000100010101100001011110000100111000001010010011010011011101010101010110100111000101100011010100000111011101011001011001110110110100110110010001100110111101001011010011110101011001101010011011100111000101100100011001010110011100110011001100000101001000110010001101110110101001100011001101100100000101101111010001100101000001111001010100100101101000110010011001110011100000101011010001010100101001001101011100000011001101101110001011110111000001100110001110010011010001101111010100110100001101001100010001010101011101101011011000110011000001101111011100110000101001101010010010000011100101000100011100010110001001001101001101100100010001010101011100000111010001110101001100110100100001001010011000100100000101010110011101110101100001010001001111010011110100001010111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111
```

Il inclut notamment cette portion :

```
010101010111001000110010011110010010101100110010011010010101101000110010010011000100010101100001011110000100111001001101001101110101010101011010011100010110001101010000011101110101100101100111011011010011011001000110011011110100101101001111010101100110101001101110011100010110010001100101011001110011001100110000010100100011001000110111011010100110001100110110010000010110111101000110010100000111100101010010010110100011001001100111001110000010101101000101010010100100110101110000001100110110111000101111011100000110011000111001001101000110111101010011010000110100110001000101010101110110101101100011001100000110111101110011011010100100100000111001010001000111000101100010010011010011011001000100010101010111000001110100011101010011001101001000010010100110001001000001010101100111011101011000010100010011110100111101
```

Qui convertie en ASCII donne :

```
Ur2y+2iZ2LEaxNM7UZqcPwYgm6FoKOVjnqdeg30R27jc6AoFPyRZ2g8+EJMp3n/pf94oSCLEWkc0osjH9DqbM6DUptu3HJbAVwXQ==
```

Elle correspond la fin du fichier chiffré **alpha.enc** obtenu dans la ligne 310 du canal alpha. Il y a donc une relation entre ces deux lignes.

Si on effectue une opération XOR entre le binaire inversé du canal alpha et le binaire inversé du dernier bit du canal rouge, on obtient :

```
0001111110001011000010000000100000011111010010111011001101011011000000000000001101000100010000010101010001000001000000000000010111000001110010010110001001000100001100000000000000000000110100001011101101011111011100010010100000010101100011001010001100011010101100011101010100010010011010111111010000110110011000110100100000010011010101000100100111001101100001001010111111101111011110111101010100111011111001000000111110101111000111101011000100101010001101110101100100011100101000101010000010101000111010101110101011101001111001011100010011111100000001011010011110000011000000010000101111010010011010111110000011011101100011011011101111010001110110010101101110001111101000010111100000101010010011010010101010001000110100110001011100101111001010111001001011101001001011001001010100111110011011011110001000110001010111111000000100001110011110011000011001111101101011101001010110101100010111011011101111110000001101001101000110001001001011100111111000011111011010110100000001110001010111110000110101101010101100000100110110010101101000010011111011100100110011010100100010110010100110011000011000111001100010010100100110111100011111100011101010111011000001001001000010110101101111000011010101100011011100101111100011100010100110010001010101011101010000100011110010001011000011010001011111000010010001110100001010000100101010100100000001011010011101101100101100000001110101011100100010010101011111110000010010011011111100111101000111011101100110011011100100100011110001001011001101101101100110111111001001001100111000110100110010011010001001100001000011100001011011111111101111011100001111001111010001000101011101010001011111110000000110110100001111101001100001111000011110000110010110111100100001111010101010100000111100111110110001101010100111000001010001011101111101110010110110001110110111100010010111001011010011110000000100011010011101010000000010110110000110001011111111101111011011101010110110100100101011111111000101000101100101010011101110010000010000000001000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111
```

Les trois premiers octets correspondent aux code hexadécimal 1F 8B 08. Intéressant car il s'agit de la [signature d'un fichier GZip](https://filesignatures.net/index.php?page=search&search=1F8B08&mode=SIG).

Nous créons donc un fichier avec ce code binaire :

```bash
echo -n $BINAIRE | perl -lpe '$_=pack"B*",$_' > fichier.gz
```

La décompression fonctionne et nous obtenons un fichier contenant :

```
U2FsdGVkX1+WPMJQISUVUvGRg7p4zCX4jIODIGb6b6cAreXFxv0WOxgCeSw9K+im
THiWMkRq45FsPXHs3TjYqcJz7QzQ8HeM340EwWQWXAi0fVy+r6NPmiJRgMgMqLCu
4Q9o/WkNyHxvPScNgG9jf8gskggx10FiTcoyF1KE+nxjmRkEuj7uQQsPrrlRP3sj
ll4KXhAzrGQZi5E4sajQOBGQfaJjei5fHXXO6sxeYsFcuxzo3JdMOF3JFYQtuUDY
```

On reconnaît la encore la chaîne `U2Fs...` caractéristique d'un fichier chiffre via OpenSSL.

On appellera ce fichier **red-lsb.enc**.

### Les courbes de Bézier ###

Appliquer un effet miroir horizontal à l'ensemble des courbes permet de relier plusieurs cellules marquées.

![Challenge](/images/310-bitcoin-challenge-curves.png "Challenge")

On se retrouve avec 5 groupes de caractères :
- L3 (ou 3L)
- 02 (ou 20)
- 584 (ou 485)
- 9F (ou F9)
- 7

A partir de la, j'ai été un peu bourrin et généré la liste complète des permutations possibles (1920) via un script Python :

```python
import itertools

sets = [ ['L3', '3L'], ['02', '20'], ['584', '485'], ['9F', 'F9'], ['7'] ]
all_item_permutations = []

sets_permutations = list(itertools.permutations(sets, len(sets)))

for sets_permutation in sets_permutations:
    item_permutations = list(itertools.product(*sets_permutation))
    all_item_permutations += item_permutations

for permutation in all_item_permutations:
    print(''.join(permutation))
```

On finit par déchiffrer les deux fichiers :

```bash
$ openssl enc -aes-256-cbc -md md5 -base64 -d -a -k "L379F48502" -in alpha.enc -out alpha.dec
$ cat alpha.dec
Bitcoin Challenge ..... 310 BTC
https://bitcoinchallenge.codes/

---

Well done!
Now find something really interesting here:


511 B20 332 328 410 530
245 651 58F C2C 03A 717
401 9AC 36A 53F 4C6 B26
332 328 410 530 491 312


---
```

```bash
$ openssl enc -aes-256-cbc -md md5 -base64 -d -a -k "02L3F95847" -in red-lsb.enc -out red-lsb.dec
$ cat red-lsb.dec
Bitcoin Challenge ..... 310 BTC
https://bitcoinchallenge.codes/

---

You're either very very close, or working in the wrong direction :)

Here you go : Z465/

---
```


La concaténation de toutes ces données nous permet d'obtenir un hash sha256 qui est accepté sur la page d'enregistrement sur laquelle pointait le QRcode :

```
$ (echo "cry buyer grain save vault sign lyrics rhythm music fury horror mansion"; cat decrypted-files/alpha.dec decrypted-files/red-lsb.dec) | tr -d '\n' | sha256sum
273e2b95648fd3cbad0d7fe3ed820e783c0b12fdbe29b57bfb2d1f243d92b1a5  -
```

![Challenge](/images/310-bitcoin-challenge-register.png "Challenge")

### Déchiffrement du second tableau ###

En utilisant la même méthode que pour le premier tableau, nous obtenons la liste de mots :

![Challenge](/images/310-bitcoin-challenge-table2-decoding.png "Challenge")

En combinant les 12 premiers mots obtenu et ces 12 nouveaux, nous obtenons les 24 mots suivants :

```
cry buyer grain save vault sign
lyrics rhythm music fury horror mansion
debris slim immune lock actual tide
gas vapor fringe pole flat glance
```

Il s'agit d'un liste BIP39 valide permettant de déverrouiller un [wallet contenant 0.2 BTC](https://www.blockchain.com/btc/address/1G7qsUy5x9bUd1pRfhVZ7cuB5cMUP4hsfR) :

- Adresse : 1G7qsUy5x9bUd1pRfhVZ7cuB5cMUP4hsfR
- Clé publique : 02707e976e321e77c6b301b14f1c64d00b2b67b11963ab739aadccba6ddca05862
- Clé privée : KxPEUpQ5BE75UGRUVjNmf8dQuWsmP9jqL3FUUjavdRW69MEcmg6C

## Pistes non concluantes (pour le moment) ##

### La trame de fond ###

Une trame de fond est détectable sur l'ensemble de l'image. On peut la mettre en évidence via une solarisation (appliquer un courbe de colorimétrique en triangle).

![Challenge](/images/310-bitcoin-challenge-pattern.png "Challenge")

Cette trame est constituée de tuiles de 128x120 pixels. Afin de l'isoler, j'ai extrait des zones sur fond blanc de l'image (principalement en haut à droite du tableau). A coups de copiers-collers et de déplacements au pixel, on obtient :

![Challenge](/images/310-bitcoin-challenge-white-pattern.png "Challenge")

Ensuite, j'ai constitué une image à partir de ce motif (dans Gimp, filtre "Tile") légèrement supérieure à la taille de l'image de départ. Je l'ai ensuite placée en couche en mode extraction de grain et l'ai calée au pixel près sur l'image de départ. Une normalisation plus tard, on obtient une image débarrassée du motif :

![Challenge](/images/310-bitcoin-challenge-pattern-removed.png "Challenge")

<iframe width="560" height="315" src="https://www.youtube.com/embed/m8C-zgHnAMk" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

En mode fusion de grain, on peut aussi récupérer la texture exacte ayant été appliquée sur l'image d'origine.

![Challenge](/images/310-bitcoin-challenge-real-pattern.png "Challenge")

Appliquée à une image random, cela donne.

![Challenge](/images/310-bitcoin-challenge-real-pattern-sample.png "Challenge")

Cela n'a abouti à rien concernant le challenge, mais c'est un skill toujours bon à prendre.

### La couche rouge ###

On a déjà prouvé que la ligne 310 de la couche rouge contenait un fichier encodé sur le dernier bit de données.

![Challenge](/images/310-bitcoin-challenge-redlayer-analysis-line.png "Challenge")

Cette image est issue de la comparaison des couches rouge et vert.

Légende :
- rouge : la valeur du canal rouge est 1 bit inférieure à celle du canal vert
- vert : la valeur du canal rouge est 1 bit supérieure à celle du canal vert
- gris : valeur du canal rouge égale à celle du canal vert
- noir : valeur du canal rouge égale à celle du canal vert, et la valeur du canal vert est à zéro
- blanc : valeur du canal rouge égale à celle du canal vert, et la valeur du canal vert est à 255 (maximum)

On remarque ici que la valeur du canal rouge varie en +1 et -1 bits par rapport au canal vert. C'est très symptomatique d'une information stockée sur le LSB d'un canal. En effet, le remplacement arbitraire du dernier bit provoque une modification de la valeur décimale comprise entre -1 et 1. En creusant un peu plus, on peut constater que ce remplacement n'est arbitraire : certaines valeurs passant par exemple de 1101111**1** à 1110000**0**, alors qu'il aurait été plus simple de mettre 1101111**0**).

En analysant en détail, on remarque trois cas :
- passage du lsb de 0 à 1 : +1
- passage du lsb de 1 à 0 (si l'octet de départ différent de 11111111) : +1
- passage du lsb de 1 à 0 (si l'octet de départ vaut 11111111) : -1

Quand on regarde le tableau du bas de l'image, on constate aussi des différences entre les canaux rouge et vert, mais d'une forme très différente :

![Challenge](/images/310-bitcoin-challenge-redlayer-analysis-table.png "Challenge")

Nous n'avons que des variations de -1 bit pour le canal rouge. Si une information est stockée ici, elle ne l'est pas de la même manière que pour la ligne 310. Le fait qu'il n'y ait pas de variation de +1 par rapport au canal de référence pourrait laisser penser qu'un LSB à 0 n'a jamais été remplacé par un 1 (+1), mais ce n'est pas le cas. On trouve des altérations du type : 1001000**0** -> 1000111**1** (qui correspond bien à -1).

On constate aussi que les variations sont beaucoup moins régulières. Ces variations ponctuelles cherchent-elles à mettre en évidence des valeurs particulières sur les pixels du canal de référence ?

## Ressources

- Site du challenge : [https://bitcoinchallenge.codes](https://bitcoinchallenge.codes)
- Conversion d'image en binaire : [https://www.dcode.fr/image-binaire](https://www.dcode.fr/image-binaire)
- Convertisseur BIP-0039: [Mnemonic Code Converter](https://iancoleman.io/bip39/#english)
- [Wallet contenant 0.1 BTC](https://www.blockchain.com/btc/address/1446C8HqMtvWtEgu1JnjwLcPESSruhzkmV)
- [Wallet contenant 0.2 BTC](https://www.blockchain.com/btc/address/1G7qsUy5x9bUd1pRfhVZ7cuB5cMUP4hsfR)
- [Wallet contenant 0.31 BTC](https://www.blockchain.com/btc/address/3NPZiNWiD7cCfXZa1D8tnEZBPgQ884cVw7)
- [Wallet contenant 310 BTC](https://www.blockchain.com/btc/address/39uAUwEFDi5bBbdBm5ViD8sxDBBrz7SUP4)
