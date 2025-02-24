---
title: "Création d'un site Web pour l'IUT de Quimper"
---
# But
Création d'un site web pour les portes ouvertes de l'IUT de Quimper.
# Cahier des charges
* Le site doit être responsive
* Il doit être intuitif
* Il doit afficher :
    * La carte de l'IUT avec des hotspots où l'on va voir les différents points d'intérêt de la journée portes ouvertes.
    * Un planning des conférences
    * La description des formations proposées
    * Les actualités
* Il doit être sécurisé (HTTPS + Certificats)
* Il doit être *<u>archivable</u>*

<u>*PS*: Je ne préciserai pas les attributs techniques du serveur car il va être publiquement accessible, donc pour des raisons de sécurité, je ne les mettrais pas ici jusqu'à la fin des portes ouvertes, auquel cas je mettrais les informations manquantes car le serveur deviendra inactif.</u>
### Spécifications techniques
J'ai opté pour l'installation de Wamp Server sur Windows Server 2022 Standard.

## <u>Pourquoi ?</u>

Comme les portes ouvertes (et donc par conséquent, le site) se passera après la fin de ma période de stage, le 1er Mars. 

Il fallait un système permettant à n'importe qui, des services concernés par l'administration des portes ouvertes, de pouvoir à la fois administrer le site de l'extérieur <mark>mais également</mark> de l'intérieur (depuis le serveur donc).

Il fallait donc un système d'exploitation accessible à tous (donc graphique) et où tout le monde peut facilement se retrouver (du Windows Server donc, pas du Linux).

Le site sera hébergé sur Wordpress (pour sa facilité de mise en service et d'usage).
# Mise en oeuvre
### WAMP Server (Windows | Apache | MySql / MariaDB | PHP)
Pour commencer, j'ai installé Windows Server 2022 et les paquets .NET Redistributables nécessaires au bon fonctionnement de Wamp Server (et donc, du site).
Par la suite, j'ai installé Wamp Server et commencé à configurer plusieurs choses :
- La base de données pour Wordpress.
- Les hôtes virtuels.
- Le fichier Host.
- Les clés de chiffrement et certificats (via Let's Encrypt) afin de sécuriser et d'authentifier le site en HTTPS.
- Les intégrations des extentions de httpd afin de prendre en compte le SSL et le HTTPS.

|{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/RDS2/JPO/cap_bdd.png" alt="Base de donnée de wordpress intégrée via PhPmyAdmin" position="center" style="border-radius: 8px;" caption="La Base de Données *wordpress* intégrée via phpmyadmin" captionPosition="right" captionStyle="color: black;" height="400" width="400">}}|{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/RDS2/JPO/cap_vhosts.png" alt="Configuration des hôtes virtuels" position="center" style="border-radius: 8px;" caption="Configuration des hôtes virtuels" captionPosition="right" captionStyle="color: black;" height="400" width="400">}}|{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/RDS2/JPO/cap_hosts.png" alt="Description du fichier Hosts" position="center" style="border-radius: 8px;" caption="Le fichier Hosts modifié (avec censure *temporaire* de l'adresse IP)" captionPosition="right" captionStyle="color: black;" height="400" width="400">}}|
|-|-|-|

|{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/RDS2/JPO/cap_SSL.png" alt="Description des hôtes virtuels pour le SSL" position="center" style="border-radius: 8px;" caption="Description des hôtes virtuels pour le SSL et le HTTPS" captionPosition="right" captionStyle="color: black;" height="400" width="400">}}|{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/RDS2/JPO/WP-encrypt/cap_pemfiles.png" alt="Fichiers PEM pour le chiffrement du site" position="center" style="border-radius: 8px;" caption="Les fichiers **PEM** pour la clé et le certificat du site" captionPosition="right" captionStyle="color: black;" height="400" width="400">}}|{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/RDS2/JPO/WP-encrypt/cap_encrypt.png" alt="Photo présentant le menu principal de l'application Let's Encrypt" position="center" style="border-radius: 8px;" caption="Capture d'écran du menu principal de l'application Let's Encrypt" captionPosition="right" captionStyle="color: black;" height="400" width="400">}}|
|-|-|-|
***
Pour une bonne configuration de Wamp Server afin qu'il puisse accepter le HTTPS, il ne fallait pas oublier cette option :
|{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/RDS2/JPO/cap_wampHTTPS.png" alt="Capture représentant le sous menu de WAMPServer, il montre ces deux options : 1) Wampserver prêt pour supporter https. 2) Autoriser HTTPS pour localhost" position="center" style="border-radius: 8px;" caption="Les deux options nécessaires au bon fonctionnement de l'HTTPS" captionPosition="right" captionStyle="color: black;">}}|
|-|
### Wordpress
Une fois WAMP et Wordpress installé, il faut maintenant configurer Wordpress.

|{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/RDS2/JPO/cap_wp-config.png" alt="Capture d'écran de la configuration de la base de donnée sur les fichiers de configurations de wordpress. Ici, on y voit toutess les informations générales du site (Nom de la base de données / mot de passe de celle-ci : nom d'hôte de la base de donnée : ici localhost car elle est sur le même ordinateur / l'encodage de la base de donnée)" position="center" style="border-radius: 8px;" caption="Capture d'écran du fichier de configuration de wp-config (Configuration de Wordpress)" captionPosition="right" captionStyle="color: black;" height="400" width="400">}}|
|-|
Oui, c'est absolument tout ce dont on à besoin pour pouvoir configurer Wordpress, outre le nom d'utilisateur et le mot de passe du panneau d'administration à mettre en place. Or *pour des raisons de sécurité* je ne les mettrais pas ici tant que l'adresse et le site sont en production.
***
Maintenant que Wordpress est configuré, il faut commencer à conçevoir la page.

Pour conçevoir la page WEB, je me suis muni de deux documents, le premier (en haut) comporte le plan de l'IUT ainsi que la légende du plan, le second (les deux en bas) est le prototype papier du design du site.
|{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/RDS2/JPO/WP-conception/JPO_plan.png" alt="" position="center" style="border-radius: 8px;" caption="" captionPosition="right" captionStyle="color: black;" height="640" width="240">}}|
|-|
|{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/RDS2/JPO/WP-conception/JPO_proto1.png" alt="" position="center" style="border-radius: 8px;" caption="Prototype du Design / Page 1" captionPosition="right" captionStyle="color: black;" height="640" width="240">}}|{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/RDS2/JPO/WP-conception/JPO_proto2.png" alt="" position="center" style="border-radius: 8px;" caption="Prototype de design / Page 2" captionPosition="right" captionStyle="color: black;" height="640" width="240">}}|
|-|-|

***
|<button onclick="window.location.href='https://vhascoet-pro.github.io/portfolio-bts.github.io/rds2/rds2_3';">Précédent</button>|<button onclick="window.location.href='https://vhascoet-pro.github.io/portfolio-bts.github.io'">Retour à l'accueil</button>|
|-|-|