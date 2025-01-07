---
title = "ID Tech Engine"
---
Ce moteur de jeux est le premier moteur de jeu à avoir pu intégrer intégralement
la 3D dans le monde de l'informatique, ce moteur à _UNE SEULE_ particularité, il
à été conçu intégralement autour d'un processeur : Le tout premier Intel Pentium
. 

{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/pentium.webp" alt="Pentium_1_pics" position="center" style="border-radius: 8px;" caption="L'un des premiers Intel Pentium" captionPosition="right" captionStyle="color: black;" >}}

Ce moteur permet d'utiliser des modèles 3D pour les personnages, les effets
visuels et pour les éclairages.

Ce moteur à des failles bient sûr, comme la physique qui est basée sur le regard
du joueur et sur le nombres d'images par secondes du jeu, il s'agit d'une
physique _pré-déterminée_, dû aux limitations du moteur de jeu, il n'est pas
possible d'y intégrer plus de 64 ennemis à la fois dans un seul niveau, le 
code-source étant basé en **C++**, il est très
bien documenté, car, comme-dit sur la page dédiée au développeur [_John Carmack_](https://vhascoet-pro.github.io/portfolio-bts.github.io/carmack),
il est l'un des acteurs majeurs du code libre (Open Source), il à mis le
code-source en ligne en Juillet 1998 (le moteur est sorti en 1997, en même temps
que le jeu Quake).

{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/quake.webp" alt="E1M1_quake" position="center" style="border-radius: 8px;" caption="Carte 1 - Episode 1 de Quake (E1M1)" captionPosition="right" captionStyle="color: black;" >}}

On peut déjà voir une améioration des graphismes, on à finalement des textures
sur chaque modèle 3D, et utilise la technologie OpenGL afin de toujours rester
open-source et compatible sur l'intégralité des plateformes de l'époque, soit
Windows 95, MS-DOS, Windows 3.x, Slackware Linux, Debian et MacOS.

Ce moteur utilise la technologie Z-Fail (ou Carmack's reverse) afin de gérer
l'intégralité des ombres du jeu, en temps réel, ce qui, avant la sortie du jeu
et du moteur 3D, n'existait pas.
***
### _BSP_

Le BSP, soit Binary Space Partitionning, est encore une technologie conçue par
John Carmack, en s'inspirant de certaines études de l'armée pour la conception
de simulateurs de vols, mais abandonnés dû aux manque de performances des
machines de l'époque.
John Carmack avait donc décidé de récupérer cet algorithme et à réussi à le
transporter sur une machine réelle.

Un système se basant sur l'algorithme BSP va utiliser ce qui s'apelle un 
"arbre BSP" qui permet de charger procéduralement une zone, qui sera scindée en
secteurs, et ces secteurs vont être simplement analysés en temps réel, ce qui va
permettre à un ordinateur de visualiser un niveau (ou une scène 3D) très vaste,
en temps réel.

![Photo BSP1](https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/BSP.png)

_Ici une scène 3D sectorisée via le BSP_
![Photo BSP3D](https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/3dtree.png)

***
# Introduction au BSP
Expliquons l'algorithme : 
Considérons un moteur de rendu 3d dans le cas où les deux faces des polygones
sont susceptibles d'être vues. On peut appliquer l'algorithme de construction
récursif suivant :

Choisir un polygone P dans la liste.

Créer un nœud **N** dans l'arbre BSP, et mettre **P** dans la liste des polygones de ce
nœud.

- Pour chaque polygone de la liste :

 - Si ce polygone est entièrement devant le plan contenant **P**, placer ce polygone
dans la liste des polygones devant **P**.

 - Si ce polygone est entièrement derrière le plan contenant **P**, placer ce
polygone dans la liste des polygones derrière **P**.

 - Si le plan contenant **P** est sécant avec le polygone, diviser le polygone en
deux par le plan de **P**, et placer chacun des demi-polygones dans la liste
adéquate (devant **P**, derrière **P**).
 - Si le polygone est coplanaire avec P, l'ajouter à la liste des polygones du
nœud **N**.
 - Appliquer cet algorithme à la liste des polygones devant **P**.

 - Appliquer cet algorithme à la liste des polygones derrière **P**.

On peut ainsi ordonner les polygones du plus éloigné vers le plus proche du
point de vue, et donc savoir dans quel ordre dessiner les polygones pour prendre
en compte les effets de masquage.

Je vais vous expliquer l'intégralité des étapes pour la génération d'un arbre
BSP :

***
# Étape 1
Prenons un espace contenant quatre segments {A, B, C, D}, admettons que cela
soit une scène en 3D, ces 4 segments seraient un polygône simple, qui
formeraient une scène.
Dans l'arbre, les "nœuds" de l'arbre BSP sont dans des cercles, et les listes
d'objets devant ou derrière seront dans des rectangles arrondis. Le côté avant
de chaque segment sera indiqué par une flèche.

![Photo BSP2](https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/BSP_steps/BSP_step1.png)
***
# Étape 2
Nous appliquons l'algorithme présenté ci-dessus :

1. Nous choisissons arbitrairement un segment, ici le A.
1. Nous créons le nœud racine contenant A.
1. Les segments sécants avec la droite portant A sont coupés ; nous avons donc
de nouveaux segments, {B1, B2, C1, C2, D1, D2}. Certains sont situés devant A,
{B2, C2, D2}, et d'autres situés derrière, {B1, C1, D1}.
1. Nous traitons, de manière récursive, d'abord les segments situés devant A
(étapes ii–v), puis ceux situés derrière (étapes vi–vii).

![Photo BSP3](https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/BSP_steps/BSP_step2.png)
***
# Étape 3
1. Nous considérons les segments situés devant A, {B2, C2, D2}.
1. Nous créons un nœud enfant, nous choisissons un segment arbitraire, B2, et
nousl'ajoutons à ce nœud.
1. Nous coupons le segment sécant avec la droite portant B2, ce qui crée deux
nouveaux segments, {D2, D3}.
1. Nous séparons les segments situés devant B2, {D2}, et ceux situés derrière,
{C2, D3}.

![Photo BSP4](https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/BSP_steps/BSP_step3.png)
***
# Étape 4
1. Nous considérons les segments situés devant B2. Il n'y a qu'un seul segment,
D2.
1. Nous créons donc un nœud enfant de B2 le contenant, il s'agit d'une feuille
de l'arbre.

![Photo BSP2](https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/BSP_steps/BSP_step4.png)
***
# Étape 5
1. Nous considérons maintenant les segments derrière B2, {C2, D3}.
1. Nous choisissons arbitrairement un segment, C2, et l'ajoutons au nœud enfant
de B2 que nous créons.
1. La liste des segments situés devant C2 est {D3}, la liste des segment situés
derrière est vide.

![Photo BSP5](https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/BSP_steps/BSP_step5.png)
***
# Étape 6
1. Nous considérons la liste des segments devant C2.
1. Le seul segment qu'elle contient, D3, est ajouté au nœud enfant de C2 que
nous créons.

![Photo BSP6](https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/BSP_steps/BSP_step6.png)
***
# Étape 7
1. Nous avons traité tous les segments situés devant A
1. Nous traitons maintenant ceux situés derrière lui.
1. Nous choisissons un segment arbitraire, B1, que nous mettons dans le nœud
enfant de A que nous créons.
1. Nous séparons les segments en deux listes : les segments situés devant B1,
{D1}, et ceux situés derrière, {C1}.

![Photo BSP7](https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/BSP_steps/BSP_step7.png)
***
# Étape 8
1. Nous traitons les segments situés devant B1.
1. Il n'y a qu'un seul segment, D1, nous créons donc un nœud enfant de B1 pour
l'y mettre.

![Photo BSP8](https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/BSP_steps/BSP_step8.png)
***
# Étape 9
1. Nous traitons les segments situés derrière B1.
1. Il n'y a qu'un seul segment, C1, nous créons donc un nœud enfant de B1 pour
l'y mettre.
1. L'arbre BSP est **_terminé_**.

![Photo BSP9](https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/BSP_steps/BSP_step9.png)
***
Cette méthode était très efficace sur des moteurs basé sur la 2D (Doom Engine)
et sur les premiers moteurs basés sur la 3D (ID Tech Engine 1, 2 Unreal Engine).
il continue toujours d'être utilisé maintenant, mais dans une grande évolution
nommée le RayTracing temps-réel, un système fonctionnant exactement de la
même manière, mais pour chaque pixel d'une scène 3D afin de calculer en temps
réel la lumière d'une scène en 3D.
***

<div align="left"><button onclick="window.location.href='https://vhascoet-pro.github.io/portfolio-bts.github.io/veille/veille_p2';">< Precédent</button>
<div align="right"><button onclick="window.location.href='https://vhascoet-pro.github.io/portfolio-bts.github.io/veille/veille_p4';">Suivant ></button>


***
[Retour au Profil](https://vhascoet-pro.github.io/portfolio-bts.github.io/about)
