---
layout: post
title: Et si Windows devenait une distribution Linux ?
tags: [Linux, Windows]
---

L'idée peut faire sourire, mais quand on y réfléchit bien, elle n'est pas si absurde que ça.
Il s'agirait bien évidemment d'une évolution à long terme, peut-être de l'ordre d'une dizaine
d'années.

Commençons par regarder la répartition des revenus de Microsoft.

![2016 Microsoft revenues](/images/2016-microsoft-revenues.jpg "2016 Microsoft revenues")

(source: [healthventures.info](https://www.healthventures.info/how-5-tech-giants-make-their-billions))

On constate en tout premier lieu que 28% des revenus sont générés par Office. En seconde
position à 22% sont regroupés Windows Server et Microsoft Azure. Je n'ai pas réussi à obtenir de chiffres plus
détaillés de ce côté là, mais je soupçonne Azure de représenter la majeure partie de cette catégorie de revenus. On sait
par ailleurs que l'[OS le plus hébergé sur Azure est Linux](https://www.zdnet.com/article/linux-now-dominates-azure/).

Aujourd'hui, même la division XBox rapporte plus à Microsoft que Windows. Ont-ils encore intérêt à assumer la
totalité du développement d'un OS pour ces 9% de revenus ? Après tout,
ils ont déjà franchi le pas pour leur navigateur web Edge [en adoptant Chromium](https://blogs.windows.com/windowsexperience/2018/12/06/microsoft-edge-making-the-web-better-through-more-open-source-collaboration/).
Qui l'aurait cru il y a encore peu (moins de dix ans), quand Internet Explorer était encore le navigateur le plus utilisé ?

En parallèle, on constate depuis ces dernières années de nombreux signes d'ouverture de Windows vers Linux, et
plus largement vers le logiciel libre.
Citons, en vrac :
* [Le portage de Microsoft SQL Server sous Linux](https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-setup?view=sql-server-2017#supportedplatforms)
* [Le développement de Visual Studio Code](https://fr.wikipedia.org/wiki/Visual_Studio_Code)
* [Le passage de Mono sous licence MIT](https://www.mono-project.com/news/2016/03/31/mono-relicensed-mit/)
* [Le rachat de Github](https://news.microsoft.com/2018/06/04/microsoft-to-acquire-github-for-7-5-billion/)
* [Adhésion de Microsoft à la Linux Foundation](https://www.cio.com/article/3141918/linux-has-won-microsoft-joins-the-linux-foundation.html)
* [L'intégration de l'interpréteur Bash dans Windows 10](https://www.zdnet.fr/actualites/microsoft-ubuntu-c-est-du-serieux-mais-pour-les-developpeurs-39834906.htm)
* [Microsoft a deux fois plus d'employés que Google contribuant à des projets open-source](https://www.infoworld.com/article/3253948/who-really-contributes-to-open-source.html)

Et tout récemment, [Microsoft à annoncé que les prochaines version de Windows contiendraient un noyau Linux maison](https://devblogs.microsoft.com/commandline/shipping-a-linux-kernel-with-windows/)
pour faire tourner WSL (Windows Subsystem for Linux). Celà fait remonter quelques souvenirs de 2011, année ou Microsoft
était [dans le top 5 des contributeurs au noyau Linux](https://www.zdnet.com/article/top-five-linux-contributor-microsoft/).

On constate aussi que Microsoft pousse très fortement les versions Cloud de ses outils (Office365), effaçant petit à petit
la dépendance à Windows. Et c'est bien normal, Windows n'est plus l'OS majoritaire. Certes il le reste sur les ordinateurs personnels
(même si MacOS X a pris une part du gâteau), mais la bataille a déjà été perdue au profit de Linux sur les serveurs, et face à Android/iOS sur
les devices mobiles qui, ne l'oublions pas, représentent aujourd'hui plus de la moitié du trafic web mondial. On pourrait même voir cette incitation à passer à "Office cloud" comme une manière d'assurer les revenus liés à Office dans l'éventualité d'un changement d'architecture de Windows.

En parallèle, on constate que Windows [n'a plus le vent en poupe auprès de développeurs](https://www.ginjfo.com/actualites/logiciels/linux/linux-detrone-windows-chez-les-developpeurs-20180323),
tant au niveau des plate-formes de développement, que des OS cibles. Ne plus attirer de développeurs, ce n'est généralement pas bon
signe sur le long terme.

Bref, Microsoft a-t-il encore intérêt à redoubler d'efforts pour maintenir Windows à flot face à la concurrence, alors que son
cœur de métier est désormais dans le cloud ? Est-il temps d'intégrer le noyau Linux dans Windows pour permettre
de porter en douceur les outils sur cette nouvelle architecture, petit à petit ? Serait-ce déjà en cours ?
