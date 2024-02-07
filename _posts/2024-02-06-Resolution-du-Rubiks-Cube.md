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

Lorsqu'on effectuera une suite de mouvements, il faudra bien veiller à conserver l'orientation globale du cube.

Quand une apostrophe (**'**) suit une lettre, celà signifie que la rotation doit être éffectuée dans le sens inverse des aiguilles d'une montre. Par exemple, **L** signifie qu'il faut faire tourner la face de gauche dans le sens des aiguilles d'une montre, **L'** indique la rotation inverse. Attention, il est facile de se tromper de sens, surtout quand on débute.

## Etape 1 : La croix blanche sur le dessus

Cette étape ne devrait pas vous poser trop de problème. Il est surtout important que chaque bordure de la croix ait la couleur correspondant à la pièce centrale de chaque face.

<div style="width: 100%, max-width: 500px; height: 500px; margin: 0px 5% 0px 5%;">
<script>
AnimCube3("bgcolor=ffffff&buttonheight=25&facelets=*w*www*w*************oo*******rr*****b**b*******gg****&position=lluu&hint=10&scale=3");
</script>
</div>

## Etape 2 : La face blanche et le premier étage

La encore, cette face peut être résolue avec des mouvements simples que je ne présenterai pas en détail.

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

Nous devons à présent apprendre nos premiers algorithmes. Le premier nous permettra de basculer la bordure de l'étage supérieur de 90° vers la gauche (sur le second étage, donc), le second nous permettra de faire de même vers la droite. On va donc tourner la face supérieur du cube pour être dans l'un des deux cas suivants.

### Bascule vers la gauche

**Algorithme : <span style="color: red">U' L' U L U F U' F'</span>**

<div style="width: 100%, max-width: 500px; height: 500px">
<script>
AnimCube3("bgcolor=ffffff&buttonheight=25&facelets=*g**y****wwwwwwwww**oooo**o**r*rr**r****g*ggg**b*bb**b&position=lldd&move=U' L' U L U F U' F'&speed=20&movetext=5&fonttype=1&textsize=20");
</script>
</div>

### Bascule vers la droite

**Algorithme : <span style="color: red">U R U' R' U' F' U F</span>**

<div style="width: 100%, max-width: 500px; height: 500px"><script>
AnimCube3("bgcolor=ffffff&buttonheight=25&facelets=*b**y****wwwwwwwww**oooo**o**r*rr**r****g*ggg**b*bb**b&position=lluu&move=U R U' R' U' F' U F&speed=20&movetext=5&fonttype=1&textsize=20");
</script>
</div>

### Autres cas

S'il n'y a plus de bordure disponible sur l'étage supérieur, on peut utiliser un des deux algorithmes pour remplacer une bordure mal placée du second étage.

## Etape 5 : Obtenir la croix jaune sur la face supérieure

### Cas du "crochet jaune"

Si vous avez un "crochet jaune" (trois cases jaunes entourant un coin) sur la face du dessus, et éventuellement des cases jaunes supplémentaires, placez le au fond à gauche et appliquez l'algorithme suivant.

**Algorithme : <span style="color: red">F U R U' R' F'</span>**

<div style="width: 100%, max-width: 500px; height: 500px"><script>
AnimCube3("bgcolor=ffffff&buttonheight=25&facelets=***yy**y*wwwwwwwww*ooyoo*oo*rr*rr*rr***gggggg*bbybb*bb&position=lluu&move=F U R U' R' F'&speed=20&movetext=5&fonttype=1&textsize=20");
</script>
</div>

### Cas de la "barre jaune"

Si vous avez une "barre jaune" (trois cases jaunes alignées, dont le centre) sur la face du dessus, et éventuellement des cases jaunes supplémentaires, placez la à l'horizontale et appliquez l'algorithme suivant.

**Algorithme : <span style="color: red">F R U R' U' F'</span>**

<div style="width: 100%, max-width: 500px; height: 500px"><script>
AnimCube3("bgcolor=ffffff&buttonheight=25&facelets=***yyy***wwwwwwwww*ooyoo*oo*rryrr*rr***gggggg*bb*bb*bb&position=lluu&move=F R U R' U' F'&speed=20&movetext=5&fonttype=1&textsize=20&hint=10&scale=3");
</script>
</div>

### Autres cas

Si vous n'êtes dans aucun de ces deux cas, appliquez un des deux algorithmes et répétez cette étape.

## Etape 6 : Terminer la face jaune

A cette étape, un seul algorithme est nécessaire (le "Sune"), mais nous allons orienter la face supérieure en fonction du motif que forment ses cases jaunes. On devra répéter cette étape autant que nécessaire (parfois 5 ou 6 fois dans les pire cas !).

**Algorithme "Sune" : <span style="color: red">R U R' U R U U R'</span>**

### Cas du "poisson jaune"

Orientez la face supérieur de manière à ce que le "poisson jaune" pointe vers le coin inférieur gauche. Si vous avez une case jaune face à vous, sur le côté droit du dernier étage, la face jaune devrait être résolue après l'exécution du "corner flipper". Sinon, orientez la face supérieure pour avoir une case jaune face à vous, sur le côté gauche ou droit du dernier étage et exécute l'algorithme. Répétez ensuite l'étape 6.

<div style="width: 100%, max-width: 500px; height: 500px"><script>
AnimCube3("bgcolor=ffffff&buttonheight=25&facelets=yy*yyy*y*wwwwwwwww*oo*ooyoo*rr*rryrr***gggggg*bb*bbybb&position=lluu&move=R U R' U R U U R'&speed=20&movetext=5&fonttype=1&textsize=20&hint=10&scale=3");
</script>
</div>

### Cas de la "croix jaune"

Si vous n'avez qu'une croix jaune sur la face supérieure, orientez cette dernière de manière à avoir une case jaune face à vous, sur le côté droit du dernier étage. Répétez ensuite l'étape 6.

<div style="width: 100%, max-width: 500px; height: 500px"><script>
AnimCube3("bgcolor=ffffff&buttonheight=25&facelets=*y*yyy*y*wwwwwwwww*oo*ooyoo*rr*rr*rr***gggggg*bb*bb*bb&position=lluu&move=R U R' U R U U R'&speed=20&movetext=5&fonttype=1&textsize=20&hint=10&scale=3");
</script>
</div>

## Etape 7 : Placer les boins coins

Si vous avez de la chance, les coins sont bien placés et vous pouvez passer à l'étape 8. Sinon, essayez de trouver une face du dernier étage ayant deux coins de la même couleur, et placez-là à l'arrière. S'il n'y en a pas, exécutez l'algorithme suivant et répétez cette étape.

**Algorithme "corner switch" : <span style="color: red">R' F R' B B R F' R' B B R R</span>**

<div style="width: 100%, max-width: 500px; height: 500px"><script>
AnimCube3("bgcolor=ffffff&buttonheight=25&facelets=yyyyyyyyywwwwwwwwwrrr*rrorrgoo*oogoob*obbbbbbbgg*ggrgg&position=lluu&move=R' F R' B B R F' R' B B R R&speed=20&movetext=5&fonttype=1&textsize=20&hint=10&scale=3");
</script>
</div>

## Etape 8 : Finir la dernière face

La dernière étape consiste à placer correctement le bordures du dernier étage. Si vous avez déjà une face terminée, vous n'êtes plus qu'à un algorithme de la résolution. Sinon, appliquez une des deux algorithmes suivants, et répétez cette étape.

Les deux algorithmes nous permettent de permuter les trois bordures des faces gauche, avant et droite. L'un permet de les faire tourner dans le sens des aiguilles d'une montre, et l'autre en sens inverse. Notez qu'appliquer deux fois de suite le même algorithme revient au même qu'appliquer l'autre. On peut ainsi éviter d'apprendre les deux.

### Rotation anti-horaire

**Algorithme : <span style="color: red">R U' R U R U R U' R' U' R R</span>**

<div style="width: 100%, max-width: 500px; height: 500px"><script>
AnimCube3("bgcolor=ffffff&buttonheight=25&facelets=*y*y*y***wwwwwwwww***o***************r**********b*****&position=lluu&move=R U' R U R U R U' R' U' R R&speed=20&movetext=5&fonttype=1&textsize=20&hint=10&scale=3");
</script>
</div>

### Rotation horaire

**Algorithme : <span style="color: red">R R U R U R' U' R' U' R' U R'</span>**

<div style="width: 100%, max-width: 500px; height: 500px"><script>
AnimCube3("bgcolor=ffffff&buttonheight=25&facelets=*y*y*y***wwwwwwwww***o***************r**********b*****&position=lluu&move=R R U R U R' U' R' U' R' U R'&speed=20&movetext=5&fonttype=1&textsize=20&hint=10&scale=3");
</script>
</div>

### Etape 9 : Admirez votre oeuvre, améliorez-vous, et retrouvez une vie normale !

<div style="width: 100%, max-width: 500px; height: 500px"><script>
AnimCube3("bgcolor=ffffff&buttonheight=25&position=lluu");
</script>
</div>

## Conclusion

Mémoriser les algorithmes peut sembler difficile. Le meilleur moyen d'y parvenir et d'inventer des moyens mnemotechniques, basés sur les mouvements à effectuer, ou sur la notation Singmaster.

Il est aussi possible de décomposer les algorithmes en groupes d'opérations. Voici quelques enchaînements fréquents, et leur appelation commune :

- "Sexy move" : **R U R' U'**
- "Inverse Sexy move" : **U R U' R'**
- "Left Sexy move" : **L' U' L U**
- "Inverse Left Sexy move": **U' L' U L**

A l'étape 4, on peut ainsi placer le cube de trois-quarts (et considérer qu'il n'y a plus de face "avant") et appliquer ces algorithmes :

- Bascule vers la gauche : "Inverse Left Sexy move" + "Inverse Sexy move"
- Bascule vers la droite : "Inverse Sexy move" + "Inverse Left Sexy move"

A l'étape 2, l'algorithme du "crocket jaune" devient : **F + "Inverse Sexy move" + F'**

Celui de la barre horizontale devient : **F + "Sexy move" + F'**

Si vous connaissez d'autres moyen mnémotechniques, n'hésitez pas à les partager en commentaires !

## Annexes

Les cubes 3D de cet article ont été générés grâce à [AnimCubeJS](https://animcubejs.cubing.net/animcubejs.html) 
