<div align="center">
<br>
<img src="https://www.utc.fr/wp-content/uploads/sites/28/2019/05/SU-UTC18-70.svg" alt="Université de Technologie de Compiègne" width="400">
<br>
<br>

# TZ - Mise en place d'une communication VoIP entre un Raspberry Pi et un téléphone IP


**Guillaume Nibert  
Encadrant : Dr. Ahmed Lounis**

<br />

[![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC_BY--SA_4.0-brightgreen.svg)](https://creativecommons.org/licenses/by-sa/4.0/)

---

<br />

English version: <a href="https://guillaume.nibert.fr/voip-asterisk-rpi-ipphone-project/" hreflang="en">https://guillaume.nibert.fr/voip-asterisk-rpi-ipphone-project/</a>

**Vous trouverez le rapport et la présentation PDF dans le dossier `/PDF`. Ces documents existent en anglais et en français**

<br />

**Rapport PDF** : <a href="../../raw/main/TZ%20-%20Guillaume%20Nibert%20-%20Rapport%20-%20Mise%20en%20place%20d'une%20communication%20entre%20un%20Raspberry%20Pi%20et%20un%20t%C3%A9l%C3%A9phone%20IP.pdf" download target="_blank">Télécharger</a>

**Présentation PDF** : <a href="../../raw/main/TZ%20-%20Guillaume%20Nibert%20-%20Pr%C3%A9sentation%20-%20Mise%20en%20place%20d'une%20communication%20entre%20un%20Raspberry%20Pi%20et%20un%20t%C3%A9l%C3%A9phone%20IP.pdf" download target="_blank">Télécharger</a>

Les annexes sont uniquement disponibles dans le rapport.

</div>

## Contexte

Télégraphe, téléphone avec liaison analogique, téléphone avec liaison numérique, téléphone IP avec des modes filaires ou non, la téléphonie représente actuellement un lien mondial très utilisé et très commode, indispensable aux échanges rapides et en temps réel, tout usage confondu. Plutôt fiable, pas trop onéreux, offrant à une cadence de plus en plus rapide des possibilités de plus en plus étendues.

Le but de ce sujet est d’utiliser le protocole IP utilisé dans l’internet, pour faire communiquer des appareils entre eux (communément appelés points de terminaison). Cette TZ mettra donc en œuvre deux points de terminaison dont les caractéristiques matérielles sont les suivantes :
 - point de terminaison 1 : téléphone Alcatel IP Touch 4018 EE ;
 - point de terminaison 2 : Raspberry Pi 3 Modèle B+.

Ci-dessous, un schéma global de l’architecture de l’infrastructure dans le réseau **192.168.1.0** *(le routeur n’est pas représenté pour des raisons de lisibilité)* :

<div align="center">
<img src="figures/figure01_schema_global.png" alt="Figure 01 - Schéma global de l'infrastructure">

*(Figure 1 - Schéma global de l’infrastructure)*

</div>

Simplement, sans rentrer dans les détails, dans ce schéma, il y a deux clients : les points de terminaison que sont l’*Alcatel IP Touch 4018 EE* et le *Raspberry Pi*. Lorsqu’il y a une communication téléphonique entre ces deux appareils, le protocole utilisé pour l’initiation de la communication est SIP *(Session Initiation Protocol)* pour la couche applicative et UDP pour la couche transport. Cette initiation passe par un intermédiaire : le serveur d'initiation SIP (Asterisk) dont l’accessibilité se fait sur la machine virtuelle Debian Asterisk Server au port 5060 ***(en bleu)***. Une fois l’initiation faite, les deux appareils se connectent entre eux directement pour laisser passer l’audio via le protocole RTP ***(en vert)***.

Un point important concerne la partie du téléphone Alcatel, qui a besoin à chaque démarrage de vérifier ses firmwares et ses fichiers de configuration lui permettant de se connecter au serveur Asterisk. Ces fichiers sont stockés dans la machine virtuelle Debian et disponibles pour l’Alcatel via le serveur HTTP au niveau du port 80 ***(en orange)***.

Ce rapport détaille l’architecture du système ainsi que la mise en place de toute l’infrastructure de téléphonie IP autorisant la communication entre ces deux appareils. Ainsi nous verrons en premier lieu quelques éléments théoriques sur les protocoles utilisés, notamment sur le rôle et le fonctionnement de SIP dans la VoIP et RTP. En deuxième lieu, nous évaluerons l’intérêt qu’il y a à utiliser le système Asterisk dans la conception de cette infrastructure et à sa mise en place, puis nous configurerons et testerons les deux clients (Raspberry Pi et téléphone Alcatel). Enfin nous concevrons un client SIP convivial en JavaScript pour le Raspberry Pi.

## [1. Protocole SIP et communication VoIP](1_sip_voip.md)

## [2. Mise en place d'un serveur PABX IP Asterisk](2_ipbx_asterisk.md)

## [3. Installation et configuration d'un client SIP sur le Raspberry Pi](3_install_client_sip_rpi.md)

## [4. Configuration du téléphone IP](4_config_alcatel.md)

## [5. Tests de communication](5_tests_com_sip.md)

## [6. Client SIP JavaScript utilisant WebRTC](6_sip_webrtc.md)

## [Conclusion](Conclusion.md)

## [Sigles](Sigles.md)

## [Références](References.md)

## [Licences utilisées](licences_utilisees.md)