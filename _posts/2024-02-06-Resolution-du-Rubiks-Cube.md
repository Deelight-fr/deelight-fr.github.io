---
layout: post
title: Résolution du Rubik's Cube
tags: [Rubik's Cube]
---

<script src="/js/AnimCube3.js"></script>

La résolution du Rubik's cube 3x3x3 est relativement simple. Elle nécessite seulement la mémorisation que quelques algorithmes (séquences de mouvements). Je vous présente ici la méthode nécessitant d'apprendre le moins d'algorithmes. Cen'est certainement pas la plus rapide mais elle vous permettra de résoudre le Rubik's cube en moins de 3 minutes.

## Introduction

Pour comprendre cette méthode, je dois tout d'abord vous présenter la terminologie utilisée.

- "Pièces centrales" : Il s'agit des pièces situées au centre de chaque face.
- "Bordure" : Ce sont les pièces situées au centre de chaque arrête du cube. Elles sont composées de deux couleurs.
- "Coins" : Ce sont les pièces situées aux sommets du cube. Elles sont composées de trois couleurs.

J'utiliserai la notation "Singmaster" (créée par [David Singmaster](https://fr.wikipedia.org/wiki/David_Singmaster)) pour décrire chaque mouvement à réaliser. A chaque face correspond une lettre :

- **F** (Front) : la face du cube qui est face à vous
- **B** (Back) : la face du cube qui est à l'arrière
- **U** (Up) : la face supérieure
- **D** (Down) : la face inférieure
- **L** (Left) : la face de gauche
- **R** (Right) : la face de droite

Lorsqu'on effectuera une suite de mouvements, il faudra bien veiller à garder la même face du cube face à nous.

Quand une apostrophe (**'**) suit une lettre, celà signifie que la rotation doit être éffectuée dans le sens inverse des aiguilles d'une montre. Par exemple, **L** signifie qu'il faut faire tourner la face de gauche dans le sens des aiguilles d'une montre, **L'** indique la rotation inverse. Attention, il est facile de se tromper de sens, surtout quand on débute.

## Etape 1 : La croix blanche sur le dessus

Cette étape ne devrait pas vous poser trop de problème. Il est surtout important que chaque bordure de la croix ait la couleur correspondant à la pièce centrale de chaque face.

<div style="width: 100%, max-width: 500px; height: 500px">
<script>
AnimCube3("bgcolor=ffffff&buttonheight=25&facelets=*w*www*w*************oo*******rr*****b**b*******gg****&position=lluu&hint=10&scale=3");
</script>
</div>

## Etape 2 : La face blanche et le premier étage

La encore, cette face peut être résolue avec des mouvements simple que je ne présenterai pas en détail.

<div style="width: 100%, max-width: 500px; height: 500px">
<script>
AnimCube3("bgcolor=ffffff&buttonheight=25&facelets=wwwwwwwww*********o**oo*o**r**rr*r**bbb*b****g**gg*g**&position=lluu&hint=10&scale=3");
</script>
</div>

## Etape 3 : On retourne le cube

<div style="width: 100%, max-width: 500px; height: 500px">
<script>
AnimCube3("bgcolor=ffffff&buttonheight=25&facelets=wwwwwwwww*********o**oo*o**r**rr*r**bbb*b****g**gg*g
**&position=lllllllllllllluuuuuuuu&hint=10&scale=3");
</script>
</div>

## Etape 4 : Résolution du deuxième étage

Nous devons à présent apprendre nos premiers algorithmes. Le premier nous permettra de basculer la bordure de l'étage supérieur de 90° vers la gauche (sur le second étage, donc), le second nous permettra de faire de même vers la droite.

### Bascule vers la gauche

**Algorithme : <span style="color: red">U' L' U L U F U' F'</span>**

<div style="width: 100%, max-width: 500px; height: 500px">
<script>
AnimCube3("bgcolor=ffffff&buttonheight=25&facelets=*g**y****wwwwwwwww**oooo**o**r*rr**r****g*ggg**b*bb**b&position=lldd&move=U' L' U L U F U' F'&speed=20&movetext=5&fonttype=0&textsize=20");
</script>
</div>

### Bascule vers la droite

**Algorithme : <span style="color: red">U R U' R' U' F' U F</span>**

<div style="width: 100%, max-width: 500px; height: 500px"><script>
AnimCube3("bgcolor=ffffff&buttonheight=25&facelets=*b**y****wwwwwwwww**oooo**o**r*rr**r****g*ggg**b*bb**b&position=lluu&move=U R U' R' U' F' U F&speed=20&movetext=5&fonttype=0&textsize=20");
</script>
</div>

### Note

Il est aussi possible d'utiliser ces algorithmes pour remplacer une pièce mal placée sur le second étage.
