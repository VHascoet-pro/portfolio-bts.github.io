---
title: "Création d'un site Web pour l'IUT de Quimper"
---
## But
Création d'un site web pour les portes ouvertes de l'IUT de Quimper.
## Cahier des charges
* Le site doit être responsive
* Il doit être intuitif
* Il doit afficher :
    * La carte de l'IUT avec des hotspots où l'on va voir les différents points d'intérêt de la journée portes ouvertes.
    * Un planning des conférences
    * La description des formations proposées
    * Les actualités
* Il doit être sécurisé (HTTPS + Certificats)
* Il doit être <u>archivable</u>

<u>*PS*: Je ne préciserai pas les attributs techniques du serveur car il va être publiquement accessible, donc pour des raisons de sécurité, je ne les mettrais pas ici.</u>
### Spécifications techniques
J'ai opté pour l'installation de Wamp Server sur Windows Server 2022 Standard.

<u>Pourquoi ?</u>

Comme les portes ouvertes (et donc par conséquent, le site) se passera après la fin de ma période de stage, le 1er Mars. 

Il fallait un système permettant à n'importe qui, des services concernés par l'administration des portes ouvertes, de pouvoir à la fois administrer le site de l'extérieur <mark>mais également</mark> de l'intérieur (depuis le serveur donc).

Il fallait donc un système d'exploitation accessible à tous (donc graphique) et où tout le monde peut facilement se retrouver (du Windows Server donc, pas du Linux).

Le site sera hébergé sur Wordpress (pour sa facilité de mise en service et d'usage).
## Mise en oeuvre
Pour Commencer, j'ai installé Windows Server 2022 et les paquets .NET Redistributables nécessaires au bon fonctionnement de Wamp Server (et donc, du site).
Par la suite, j'ai installé Wamp Server et commencé à configurer plusieurs choses :
- La base de données pour Wordpress
- Les hôtes virtuels

|{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/RDS2/JPO/cap_bdd.png" alt="Base de donnée de wordpress intégrée via PhPmyAdmin" position="center" style="border-radius: 8px;" caption="La Base de Données *wordpress* intégrée via phpmyadmin" captionPosition="right" captionStyle="color: black;" >}}|{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/RDS2/JPO/cap_vhosts.png" alt="Configuration des hôtes virtuels" position="center" style="border-radius: 8px;" caption="Configuration des hôtes virtuels" captionPosition="right" captionStyle="color: black;" >}}|
|-|-|
## Conclusion
[Lien](https://portesouvertes-iutq.univ-brest.fr) vers la page web.
***
|<button onclick="window.location.href='https://vhascoet-pro.github.io/portfolio-bts.github.io/rds2/rds2_3';">Précédent</button>|<button onclick="window.location.href='https://vhascoet-pro.github.io/portfolio-bts.github.io'">Retour à l'accueil</button>|
|-|-|