---
title: "Troisième Partie - Installation de FOG"
---

Dans cette partie, j'avais proposé l'installation d'un service d'installation et de configuration d'un serveur FOG, permettant de déployer des images disques sur des ordinateurs afin de faciliter le déploiement d'une flotte d'ordinateurs avec des images de n'importe quel système possible.

Pour faire fonctionner le serveur Fog ici il faut : 
- Un serveur DHCP
- Le serveur FOG qu'il faut ensuite configurer

Pour déployer l'image d'un PC depuis le serveur FOG vers le PC en question, il faut d'abord avoir capturé cette-dite image depuis soit une machine virtuelle, soit une machine physique que l'on aura configuré plus tôt.

Pour capturer une machine, il faut l'enregistrer sur le serveur FOG une fois qu'il est configuré.


J'ai donc commencé le TP par la conception d'un schéma réseau (comme à l'accoutumée, sur le logiciel Packet Tracer)

{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/RDS/sch_réseau-RDS2-FOG.png" alt="schéma réseau" position="center" style="border-radius: 8px;" caption="" captionPosition="right" captionStyle="color: black;" >}}

***
|<button onclick="window.location.href='https://vhascoet-pro.github.io/portfolio-bts.github.io/rds2/rds2_2';">Précédent</button>|<button onclick="window.location.href='https://vhascoet-pro.github.io/portfolio-bts.github.io/rds2/rds2_4';">Suivant</button>|
|-|-|