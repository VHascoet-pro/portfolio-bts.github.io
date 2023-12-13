+++
title = "Unreal Engine"
draft = false
+++
En 1994, l'ingénieur Tim Sweeney au studio Epic MegaGames voudra faire un jeu de 
flipper en 3D pour en faire une démo technique qui lui permettra de tester ses
compétences dessus, dont un système permettant d'afficher et d'étirer des
textures afin de fausser de la 3D, c'est du _*parralax*_, il se mettra au
travail et réussira à sortir, en 1995 : Un moteur 3D innovant qui permettra de
faire de la simulation de fluides ( très primitive selon nos standards de
maintenant ), des systèmes de brouillards ambiant, des effets utilisant des
voxels, des reflets dynamiques.

Voilà la démo technique en question :

{{<video src="https://vhascoet-pro.github.io/portfolio-bts.github.io/vids/UE1_flyby_intro.mp4" height="640" width="640">}}

## Qu'est-ce qu'un voxel ?
***

Un Voxel est, en quelque sorte, un pixel en trois dimensions, en voici un :
{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/voxel_example.webp" alt="Exemple de Voxel" position="center" style="border-radius: 8px;" caption="Voici un voxel" captionPosition="right" captionStyle="color: black;" >}}

Il s'agit d'un type de pixel qui permet de générer des scènes en 3D "sans"
polygônes, cela permettait à une certaine époque, d'avoir des modèles 3D sans
avoir aucune vertrice, car cela est considéré comme un pixel normal.
***
Le moteur permet également d'avoir des modèles avec des squelettes d'animations,
il s'agit du système d'animation désormais le plus utilisé, mais qui était
révolutionnaire à l'époque, car avant, il fallait avoir une liste de positions
pour chaque animation de chaque modèle 3d.

Maintenant, il suffit d'un seul fichier d'animations lié à un modèle,
qui contient toutes les animations d'un seul modèle dans un seul fichier 
( optimisant 2x mieux les rendus, car cela est simplement plus simple d'avoir
des animations lié à un modèle, plutôt que de remodeler chaque modèle pour
chaque animation ).

{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/skeleton_bodies.webp" alt="Exemple de Voxel" position="center" style="border-radius: 8px;" caption="Voici un squelette sur Unreal" captionPosition="right" captionStyle="color: black;" >}}

Avoir un système de squelette d'animation permet de créer plus efficacement des
animations sans avoir à utiliser beaucoup d'espace (comparé à l'ancien système
d'animation introduit par ID Software avec son moteur ID Tech Engine 1 dans le
jeu quake, dans lequel les animations étaient faites en bougeant des groupes de
vertrices image par image, pour donner une animation plutôt fluide mais qui
était très mal implémentable avec l'évolution des ordinateurs de l'époque et
donc, du nombre de vertrices possibles par modèle 3D )
***
<div align="left"><button onclick="window.location.href='https://vhascoet-pro.github.io/portfolio-bts.github.io/veille/veille_p3';">< Precédent</button></div> 
<div align="right"><button onclick="window.location.href='https://vhascoet-pro.github.io/portfolio-bts.github.io/veille/veille_p5';">Suivant ></button></div>
