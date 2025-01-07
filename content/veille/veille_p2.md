---
title = "Wolfenstein Tech Engine"
---
Le _premier_ moteur de jeu open-source.

Voici le tout premier moteur de jeux open-source de l'histoire d'Internet.

Conçu par [_John Carmack_](https://en.wikipedia.org/wiki/John_Carmack) en 1991, ce moteur de jeu est le tout premier moteur ayant le pouvoir d'offrir des plafonds au format bitmap.
{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/Wolf_Engine.webp" alt="Wolfenstein 3D COVER" position="center" style="border-radius: 8px;" caption="Démonstration du moteur de John Carmack" captionPosition="right" captionStyle="color: black;" >}}

Ce moteur de jeux est très important car il permettra à [_ID Software_](https://en.wikipedia.org/wiki/Id_Software) de se lancer dans le monde du jeu vidéo, on va voir plus tard pourquoi ce studio va être très important par la suite dans la conception et la programmation des moteurs de jeux, ils ont permi l'accélération de l'ingénierie du processeur par Intel, ils ont "forcé la main" à Intel afin de créer des processeurs permettant de calculer de la 3D en temps réel plus vite que la concurrence.


Sur cette image de couverture du jeu, on y voit une capture d'écran du jeu, on y voit deux problèmes, d'abord, le plafond et le sol sont gris, et les murs sont tous tournés en angles droits, il s'agit d'une limitation technologique du aux processeurs de l'époque, le jeu est sorti en 1992, sur des processeurs i386 de l'époque, il n'était pas possible techniquement d'avoir des textures aux plafonds et au sol.

C'est également le tout premier moteur de jeux à utiliser le langage **C** pour son developpement, comme le langage était encore sous-documenté, John Carmack à dû créer des fonctions uniques à ce langage, dont une fonction permettant de calculer une racine carrée inverse, la voici :

```c
float Q_rsqrt( float number ){
long i;
float x2, y;
const float threehalfs = 1.5F;
    
x2 = number * 0.5F;
y  = number;
i  = * ( long * ) &y; // evil floating point bit level hacking
i  = 0x5f3759df - ( i >> 1 ); // what the f**k?
y  = * ( float * ) &i;
y  = y * ( threehalfs - ( x2 * y * y ) ); // 1st iteration
// y = y * ( threehalfs - ( x2 * y * y ) ); // 2nd iteration, this can be removed
    
#ifndef Q3_VM
#ifdef __linux__
  assert( !isnan(y) ); // bk010122 - FPE?
#endif
#endif
return y;
}
```


Il s'agit d'une fonction qui s'avèrera 4 fois plus rapide que la fonction

```c
(float) 1.0/sqrt(x)
```

Qui est donc plus optimisée et est plus adaptée aux jeux vidéos.
***
# _Le Raycasting_
Cette technique d'affichage à été la toute première technique d'affichage de "3D" (je le met ce mot entre guillemets car il s'agit d'une fausse 3D, on va en parler).

Le raycasting (lancer de rayons) est une technique de rendu dans les jeux vidéo pour, à partir d'une carte en 2D, afficher en temps réel un environnement qui à l'apparence d'une 3D rudimentaire.

Le principal avantage de cette technique est d'être rapide et abordable en termes de calcul temps réel pour les ordinateurs du début des années 90.

Cette technique est également appelée parfois « 2.5D » pour la différencier des premiers moteurs de rendu en vraie 3D, comme le moteur de Quake, mais cette appellation recouvre également d'autres techniques de représentation d'un environnement 3D en 2D comme les projections axonométriques dont la perspective isométrique.

_Il ne faut pas confondre raycasting et raytracing :_

Le raytracing est une autre technique de rendu qui est beaucoup plus coûteuse en temps de calcul et simule le parcours des rayons de lumière de différentes sources pour éclairer la scène en essayant d'atteindre une qualité photoréaliste.

Depuis la fin 2019, la puissance des cartes graphiques est telle que certains proposent du raytracing en temps réel.

Notons enfin que si le raycasting est rapide sur les résolutions de l'écran de l'époque, 320x200, 640x400 ou 640x480, ses performances sont très mauvaises sur nos grandes résolutions modernes et il vaut mieux passer par des techniques plus modernes comme les API 3D OpenGL (open-source), Vulkan (évolution d'OpenGL) ou DirectX (Propriété de Microsoft).

# Bref historique de cette technologie
Le moteur qui implémente le raycasting dans Wolfenstein 3D propose néanmoins des fonctionnalités très rudimentaires : le niveau est stocké dans un tableau, une grille.

La valeur d'une case indique si la case est pleine ou creuse.

Tous les murs ont la même hauteur, ont une longueur qui est toujours un multiple d'une valeur de base, ils se croisent toujours perpendiculairement, le sol et le plafond ne sont pas texturés, la lumière est statique.

Le moteur offre néanmoins des murs texturés, des portes, des sprites pour les ennemis, les objets et les lumières statiques.

_Avant le rendu :_
{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/WOLF3D_MAP.webp" alt="map_ed" position="center" style="border-radius: 8px;" caption="Exemple d'une carte dans l'éditeur de Wolfenstein 3D" captionPosition="right" captionStyle="color: black;" >}}

_Après le rendu :_
{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/wolf3d_screenshot.webp" alt="Ingame_scr" position="center" style="border-radius: 8px;" caption="Capture d'écran in-game de Wolfenstein 3D" captionPosition="right" captionStyle="color: black;" >}}
***

**À la page suivante, on va parler du troisième grand moteur de jeux qui révolutionnera le jeux vidéo et le monde de la 3D en général (il à aussi été créé par John Carmack)**

<div align="left"><button onclick="window.location.href='https://vhascoet-pro.github.io/portfolio-bts.github.io/veille/veille';">< Precédent</button>
<div align="right"><button onclick="window.location.href='https://vhascoet-pro.github.io/portfolio-bts.github.io/veille/veille_p3';">Suivant ></button>

***