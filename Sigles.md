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

## [Conclusion](Conclusion.md)

## Sigles

| Sigle   | Description                                                                                                                                                                                                                       |
| ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 802.11  | Norme IEEE 802.11 (Wi-Fi).                                                                                                                                                                                                        |
| 802.3   | Norme IEEE 802.3 (Ethernet).                                                                                                                                                                                                      |
| ANSSI   | Agence nationale de la sécurité des systèmes d'information.                                                                                                                                                                       |
| ECDSA   | Elliptic curve digital signature algorithm, algorithme asymétrique de signature numérique utilisant la cryptographie sur les courbes elliptiques.                                                                                 |
| FTP     | File Transfer Protocol.                                                                                                                                                                                                           |
| HDMI    | High-Definition Multimedia Interface.                                                                                                                                                                                             |
| HTTP    | Hypertext Transfer Protocol.                                                                                                                                                                                                      |
| HTTPS   | HyperText Transfer Protocol Secure.                                                                                                                                                                                               |
| IEEE    | Institute of Electrical and Electronics Engineers.                                                                                                                                                                                |
| IP      | Internet Protocol, couche réseau du modèle TCP/IP.                                                                                                                                                                                |
| IPBX    | Internet Protocol Private Branch eXchange, c'est un PABX fonctionnant sur la pile internet (aussi appelé PABX-IP).                                                                                                                |
| ISDN    | Integrated Services Digital Network : RNIS.                                                                                                                                                                                       |
| LTS     | Long Term Support, version d'un logiciel/système supportée à long terme.                                                                                                                                                          |
| MAC     | Media Access Control, protocole de la couche liaison du modèle OSI.                                                                                                                                                               |
| NAT     | Network Address Translation.                                                                                                                                                                                                      |
| P-521   | Algorithme de chiffrement utilisant des courbes elliptiques développé par la National Institute of Standards and Technology.                                                                                                      |
| PABX    | Private Automatic Branch eXchange (Autocommutateur téléphonique privé), permet de relier les points de terminaison téléphoniques.                                                                                                 |
| PABX-IP | cf. IPBX.                                                                                                                                                                                                                         |
| PBX     | Private Branch eXchange, cf. PABX.                                                                                                                                                                                                |
| PHY     | Couche physique du modèle OSI.                                                                                                                                                                                                    |
| PoE     | Power over Ethernet, norme IEEE 802.3af, un périphérique PoE permet d'alimenter un périphérique via Ethernet tout en conservant la capacité à transférer des données.                                                             |
| RFC     | Request for comments, document officiel spécifiant des technologies de l'Internet.                                                                                                                                                |
| RNIS    | Réseau Numérique à Intégration de Services, réseau téléphonique numérique avec des débits pouvant atteindre 2 Mbit/s ([Wikipédia](https://fr.wikipedia.org/wiki/R%C3%A9seau_num%C3%A9rique_%C3%A0_int%C3%A9gration_de_services)). |
| RPi     | Raspberry Pi.                                                                                                                                                                                                                     |
| RSA     | "Chiffrement RSA (nommé par les initiales de ses trois inventeurs : Ronald Rivest, Adi Shamir et Leonard Adleman) est un algorithme de cryptographie asymétrique" - [Wikipédia](https://fr.wikipedia.org/wiki/Chiffrement_RSA).   |
| RTC     | Réseau Téléphonique Commuté, réseau téléphonique analogique.                                                                                                                                                                      |
| RTP     | Real-time Transport Protocol, protocole applicatif permettant entre autres le transfert de flux audio ou vidéo.                                                                                                                   |
| SIP     | Session Initiation Protocol, protocole établissant la communication VoIP entre deux points de terminaison.                                                                                                                        |
| SIPS    | Session Initiation Protocol over SSL/TLS.                                                                                                                                                                                         |
| SMTP    | Simple Mail Transfer Protocol.                                                                                                                                                                                                    |
| SRTP    | Secure Real-time Transport Protocol.                                                                                                                                                                                              |
| SSH     | Secure Shell, protocole de communication applicatif.                                                                                                                                                                              |
| SSL     | Secure Socket Layer, protocole de sécurisation des échanges.                                                                                                                                                                      |
| TCP     | Transmission Control Protocol, protocole de la couche transport du modèle OSI (mode connecté).                                                                                                                                    |
| TFTP    | Trivial File Transfer Protocol, protocole applicatif permettant le transfert de fichiers par UDP.                                                                                                                                 |
| TLS     | Transport Layer Security, successeur de SSL.                                                                                                                                                                                      |
| UDP     | User Datagram Protocol, protocole de la couche transport du modèle OSI (mode non connecté).                                                                                                                                       |
| VM      | Virtual Machine (machine virtuelle).                                                                                                                                                                                              |
| VoIP    | Voice over Internet Protocol.                                                                                                                                                                                                     |
| WebRTC  | Web Real-Time Communication. “Interface de programmation (API) JavaScript développée au sein du W3C et de l'IETF." - [Wikipédia](https://fr.wikipedia.org/wiki/WebRTC)                                                            |
| WS      | "L'API WebSocket est une technologie évoluée qui permet d'ouvrir un canal de communication bidirectionnelle entre un navigateur (côté client) et un serveur."                                                                     |
| WSS     | Web Socket Secure.                                                                                                                                                                                                                |