---
layout: post
title: Résolution du Rubik's Cube
tags: [Rubik's Cube]
---

<script src="/js/AnimCube3.js"></script>

## Résolution du deuxième étage

Nous devons à présent apprendre nos premiers algorithmes. Le premier nous permettra de basculer la pièce centrale de l'étage supérieur de 90° vers la gauche (sur le second étage, donc), le second nous permettra de faire de même vers la droite.

### Bascule vers la gauche

**Algorithme : U' L' U L U F U' F'**

<div style="width: 100%, max-width: 500px; height: 500px">
<script>
AnimCube3("bgcolor=ffffff&buttonheight=25&facelets=*b**y****wwwwwwwww**oooo**o**r*rr**r****b*bbb**g*gg**g&position=lldd&move=U' L' U L U F U' F'&speed=20");
</script>
</div>

### Note

Il est aussi possible d'utiliser ces algorithmes pour remplacer une pièce mal placée sur le second étage.
