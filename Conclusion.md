<div align="center">
<br>
<img src="https://www.utc.fr/wp-content/uploads/sites/28/2019/05/SU-UTC18-70.svg" alt="Université de Technologie de Compiègne" width="400">
<br>
<br>

# TZ - Mise en place d'une communication VoIP entre un Raspberry Pi et un téléphone IP


**Guillaume Nibert  
Encadrant : Dr. Ahmed Lounis**

</div>

## [Contexte](README.md)

## [1. Protocole SIP et communication VoIP](1_sip_voip.md)

## [2. Mise en place d'un serveur PABX IP Asterisk](2_ipbx_asterisk.md)

## [3. Installation et configuration d'un client SIP sur le Raspberry Pi](3_install_client_sip_rpi.md)

## [4. Configuration du téléphone IP](4_config_alcatel.md)

## [5. Tests de communication](5_tests_com_sip.md)

## [6. Client SIP JavaScript utilisant WebRTC](6_sip_webrtc.md)

## Conclusion

Ce projet a permis l’élaboration d’une communication en VoIP entre un Raspberry Pi et un téléphone IP Alcatel IP Touch 4018 EE. Nous avons découvert de nouveaux protocoles tels que SIP ou RTP et l’API WebRTC permettant d’encapsuler SIP pour établir une communication entre plusieurs points de terminaisons, utilisant un navigateur web ou non. On se rend bien compte du potentiel de l’IP pour les communications, et de l’indispensabilité de l’utilisation de ce type de technologie dans les entreprises ou services publics.

Ce projet pourrait être amélioré par la mise en place de chiffrement notamment avec le protocole SIPS (SIP over SSL/TLS) au niveau de l’établissement de la communication entre deux points de terminaison (fait en partie lorsqu’il est encapsulé dans une WebSocket over TLS), mais cela pourrait être intégral, puisque l’Alcatel supporte très bien SIP over TLS. Enfin, le protocole RTP peut aussi être chiffré, il s’agit du protocole SRTP (*Secure Real-time Transport Protocol*) lorsqu’il y a un appel en cours.

Je remercie Monsieur Lounis de m’avoir proposé cette TZ, particulièrement enrichissante, j’ignorais une grande partie du fonctionnement de la téléphonie sur IP. Ce sujet m’a permis de manipuler à la fois l’aspect back end et front end du système et de mesurer la puissance de cette architecture lorsqu’elle est intégrée dans le réseau de l’Internet.

## [Sigles](Sigles.md)

## [Références](References.md)