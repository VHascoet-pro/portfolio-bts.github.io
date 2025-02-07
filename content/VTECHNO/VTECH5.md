---
title: "Cry Engine"
---
En 2003, l'entreprise polonaise, nouvellement crée. CryTek se proposa pour améliorer le moteur de jeux [Unreal Engine 2](https://vhascoet-pro.github.io/portfolio-bts.github.io/veille/veille_p4) afin de lui doter :
- D'une Simulation de fluides beaucoup plus réaliste
- D'un Système de Méga-Textures
- D'une limitation de terrain drastiquement plus élevée
- Du tout premier moteur permettant le SLI et le Crossfire (technologie de nVIDIA et d'AMD permettant de faire fonctionner jusqu'à 4 cartes graphiques en simultané sur la même carte mère, afin de partager la VRAM. Ces systèmes utilise une nappe permettant la synchronisation de ces cartes, qui se connectait sur le dessus de la carte).

# Les Méga-Textures
<u>Qu'est-ce qu'une Texture ?</u>

Il s'agit d'un système d'affichage utilisant des images que l'on peut "coller" sur des objets en 3D, par exemple, on génère un pavé, il n'aura aucune couleur, il sera tout simplement transparent et n'aura aucune collision (c'est à dire que l'entité, que ce soit une caméra virtuelle pour une scène de cinéma, un joueur ou un personnage non joueur pourra traverser l'objet, on dit qu'il n'à **aucune propriété**).

Sur cet objet, je peux coller une image sur chaque face du pavé, cela permet de le rendre visible.

Or, une texture, comme une texture est une image simple (jpg, png ou gif), ce qui veut dire qu'elle à une taille maximale, donc, l'utiliser sur un pavé plus **grand** que la taille de l'image (en longueur et en largeur) va faire **boucler l'image**, elle va se répéter litérallement.

Mais aussi, dans un moteur de jeu classique sorti AVANT le **CryEngine**, une texture avait une taille maximale (exactement de 1024p par 1024p, le 'p' étant le nombre de 'pixels' de long ou de large, c'est une longueur arbitraire qui n'est pas definissable dans le monde palpable).

<u>Qu'est-ce qu'une Méga-Texture ?</u>

Une Méga-texture est, en toute logique, une texture beaucoup plus grande que la normale, elle permet d'avoir une texture de maximum 100Mp² (100 Millions de pixels carrés), c'est à dire des textures **Cent mille** fois plus grandes que la normale.

<u>En quoi une Méga-Texture est utile ?</u>

Pour faire des terrains, imaginez, faire une ville de plusieurs dizaines de kilomètres carrés, ou l'on peut appliquer une seule texture sur l'intégralité des bâtiments de la ville, des routes, des arbres.

### Exemple

Le jeu Rage, développé par ID Software en utilisant CryEngine, avait une texture si grande qu'elle faisait une taille totale de : 1 Téra-Octet. le jeu à donc été compressé de telle manière que cette même texture fasse seulement 8 Go.
Ce qui à permi de :
1. Avoir une carte qui se charge en seulement 3 secondes (sous-zones comprises) au lieu de 10.21 secondes pour la carte non compressée et une moyenne de 15 secondes pour une carte standard.
2. Avoir un jeu qui ne pert jamais en qualité visuelle quelle que soit la distance du joueur aux modèles.

***
|<button onclick="window.location.href='https://vhascoet-pro.github.io/portfolio-bts.github.io/vtechno/vtech4';">Précédent</button>|<button onclick="window.location.href='https://vhascoet-pro.github.io/portfolio-bts.github.io/vtechno/vtech6';">Suivant</button>|
|-|-|