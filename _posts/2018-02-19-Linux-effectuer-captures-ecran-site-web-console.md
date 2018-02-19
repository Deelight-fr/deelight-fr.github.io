---
layout: post
title: Linux / Effectuer des captures d'écran de sites web en console via cutycapt
tags: [Linux]
---

Cutycapt est un outil basé sur Qt qui permet d'effectuer des captures d'écran de sites web. Par défaut il capture l'intégralité de la page mais on peut facilement cropper et redimensionner l'image générée pour obtenir une vignette de prévisualisation d'un site web.

```bash
while read a
do
        cutycapt --url=http://$a --out=$a.png
        convert -resize 240 -crop 240x180+0+0 $a.png $a.png
done < url-list.txt
```
