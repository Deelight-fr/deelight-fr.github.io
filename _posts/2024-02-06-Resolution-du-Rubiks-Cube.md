---
layout: post
title: Résolution du Rubik's Cube
tags: [Rubik's Cube]
---

<script src="/js/AnimCube3.js"></script>

La résolution du Rubik's cube 3x3x3 est relativement simple. Elle nécessite seulement la mémorisation de quelques algorithmes (séquences de mouvements). Je vous présente ici la méthode nécessitant d'apprendre le moins d'algorithmes. Ce n'est certainement pas la plus rapide mais elle vous permettra de résoudre le Rubik's cube en moins de 3 minutes.

## Introduction

Pour comprendre cette méthode, je dois tout d'abord vous présenter la terminologie utilisée.

- **Pièces centrales** : Il s'agit des pièces situées au centre de chaque face.
- **Bordures** : Ce sont les pièces situées au milieu de chaque arrête du cube. Elles sont composées de deux couleurs.
- **Coins** : Ce sont les pièces situées aux sommets du cube. Elles sont composées de trois couleurs.

J'utiliserai la notation "Singmaster" (créée par [David Singmaster](https://fr.wikipedia.org/wiki/David_Singmaster)) pour décrire chaque mouvement à réaliser. A chaque face correspond une lettre :

- **F** (Front) : la face du cube qui est face à vous
- **B** (Back) : la face du cube qui est à l'arrière
- **U** (Up) : la face supérieure
- **D** (Down) : la face inférieure
- **L** (Left) : la face de gauche
- **R** (Right) : la face de droite

Lorsqu'on effectuera une suite de mouvements, il faudra bien veiller à garder la même face du cube face à soi.

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
AnimCube3("bgcolor=ffffff&buttonheight=25&facelets=wwwwwwwww*********o**oo*o**r**rr*r**bbb*b****g**gg*g**&position=lllllllllllllluuuuuuuu&hint=10&scale=3");
</script>
</div>

## Etape 4 : Résolution du deuxième étage

Nous devons à présent apprendre nos premiers algorithmes. Le premier nous permettra de basculer la bordure de l'étage supérieur de 90° vers la gauche (sur le second étage, donc), le second nous permettra de faire de même vers la droite. On va donc tourner la face supérieur du cube pour être dans l'un des dexu cas suivants.

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

### Autres cas

S'il n'y a plus de bordure disponible sur l'étage supérieur, on peut utiliser un des deux algorithmes pour remplacer une bordure mal placée du second étage.

## Etape 5 : Obtenir la croix jaune sur la face supérieure

### Cas du "crochet jaune"

Si vous avez un "crochet jaune" (trois cases jaunes entourant un coin) sur la face du dessus, et éventuellement des cases jaunes supplémentaires, placez le au fond à droite et appliquez l'algorithme suivant.

**Algorithme : <span style="color: red">F U R U' R' F'</span>**

<div style="width: 100%, max-width: 500px; height: 500px"><script>
AnimCube3("bgcolor=ffffff&buttonheight=25&facelets=***yy**y*wwwwwwwww*ooyoo*oo*rr*rr*rr***gggggg*bbybb*bb&position=lluu&move=F U R U' R' F'&speed=20&movetext=5&fonttype=0&textsize=20&hint=10&scale=3");
</script>
</div>

### Cas de la "barre jaune"

Si vous avez une "barre jaune" (trois cases jaunes alignées, dont le centre) sur la face du dessus, et éventuellement des cases jaunes supplémentaires, placez la à l'horizontale et appliquez l'algorithme suivant.

**Algorithme : <span style="color: red">F R U R' U' F'</span>**

<div style="width: 100%, max-width: 500px; height: 500px"><script>
AnimCube3("bgcolor=ffffff&buttonheight=25&facelets=***yyy***wwwwwwwww*ooyoo*oo*rryrr*rr***gggggg*bb*bb*bb&position=lluu&move=F R U R' U' F'&speed=20&movetext=5&fonttype=0&textsize=20&hint=10&scale=3");
</script>
</div>

### Autres cas

Si vous n'êtes dans aucun de ces deux cas, appliquez un des deux algorithmes et répétez cette étape.

## Etape 6 : Terminer la face jaune

A cette étape, un seul algorithme est nécessaire (le "corner flipper"), mais nous allons orienter la face supérieure en fonction du motif que forment ses cases jaunes. On devra répéter cette étape autant que nécessaire (parfois 5 ou 6 fois dans les pire cas !).

**Algorithme "corner flipper" : <span style="color: red">R U R' U R U U R'</span>**

### Cas du "poisson jaune"

Orientez la face supérieur de manière à ce que le "poisson jaune" pointe vers le coin inférieur gauche. Si vous avez une case jaune face à vous, sur le côté droit du dernier étage, la face jaune devrait être résolue après l'exécution du "corner flipper". Sinon, orientez la face supérieure pour avoir une case jaune face à vous, sur le côté gauche ou droit du dernier étage. Vous devrez ensuite répéter cette étape autant que nécessaire.

<div style="width: 100%, max-width: 500px; height: 500px"><script>
AnimCube3("bgcolor=ffffff&buttonheight=25&facelets=yy*yyy*y*wwwwwwwww*oo*ooyoo*rr*rryrr***gggggg*bb*bbybb&position=lluu&move=R U R' U R U U R'&speed=20&movetext=5&fonttype=0&textsize=20&hint=10&scale=3");
</script>
</div>

### Cas de la "croix jaune"

Si vous n'avez qu'une croix jaune sur la face supérieure, orientez cette dernière de manière à avoir une case jaune face à vous, sur le côté droit du dernier étage.

<div style="width: 100%, max-width: 500px; height: 500px"><script>
AnimCube3("bgcolor=ffffff&buttonheight=25&facelets=*y*yyy*y*wwwwwwwww*oo*ooyoo*rr*rr*rr***gggggg*bb*bb*bb&position=lluu&move=R U R' U R U U R'&speed=20&movetext=5&fonttype=0&textsize=20&hint=10&scale=3");
</script>
</div>


