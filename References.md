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

## [Conclusion](Conclusion)

## [Sigles](Sigles.md)

## Références

<a name="wikipedia_pabx"></a>**(1)** : Wikipédia, *Généralités* **In** : *Autocommutateur téléphonique privé*, 26 novembre 2020, Disponible sur : https://fr.wikipedia.org/wiki/Autocommutateur_t%C3%A9l%C3%A9phonique_priv%C3%A9.

<a name="q931"></a>**(2)** : Secteur de la normalisation des télécommunications de l'UIT, *Spécification de la couche 3 de l'interface utilisateur-réseau RNIS pour la commande de l’appel de base*, UIT, Mai 1998, Disponible sur : https://www.itu.int/rec/T-REC-Q.931-199805-I/fr.

<a name="x253"></a>**(3)** : Secteur de la normalisation des télécommunications de l'UIT, *Interface entre équipement terminal de traitement de données et équipement de terminaison de circuit de données pour terminaux fonctionnant en mode paquet et raccordés par circuit spécialisé à des réseaux publics pour données*, UIT, Octobre 1996, Disponible sur : https://www.itu.int/rec/T-REC-X.25-199610-I/fr.

<a name="sip_deprecie"></a>**(4)** : Matt Fredrickson, *PSA: chan_sip status changed to “deprecated” & Asterisk 17.0.0-rc2 Release*, Asterisk.org, 25 septembre 2019, Disponible sur : https://www.asterisk.org/deprecating-chan_sip-asterisk-17-0-0-rc2-release/.

<a name="problematique_traversee_nat"></a>**(5)** : Odin Gremaud, *Introduction* **In** : *La traversée de NAT en VoIP SIP*, NEXCOM Systems, p.1, 20 juin 2012, Disponible sur : https://www.nexcom.fr/wp-content/uploads/whitepapers/bcp_nat_traversal.pdf.

<a name="alaw_ulaw_alcatel"></a>**(6)** : Alcaltel-Lucent, *Caractéristiques audio* **In** : *Téléphones Alcatel-Lucent IP Touch 4008/4018 Extended Edition*, p.2, 2010, Disponible sur : https://www.bureautique-communication.fr/fileuploader/download/download/?d=0&file=custom%2Fupload%2F1%2F6%2F16590.pdf.

<a name="alaw_ulaw_geo"></a>**(7)** : Wikipédia, *G.711*, 17 juillet 2017, Disponible sur : https://fr.wikipedia.org/wiki/G.711.

<a name="extensions_conf"></a>**(8)** : Malcolm Davenport, *Answer, Playback, and Hangup Applications* **In** : *Asterisk Documentation*, Asterisk.org, 19 décembre 2013, Disponible sur : https://wiki.asterisk.org/wiki/display/AST/Answer%2C+Playback%2C+and+Hangup+Applications.

<a name="rpi_benchmark"></a>**(9)** : bluesman, *64 bit Raspberry Pi OS is here!*, Audiophile Style, 4 juin 2020, Disponible sur : https://audiophilestyle.com/forums/topic/59499-64-bit-raspberry-pi-os-is-here/.

<a name="alcatel_conf"></a>**(10)** : Alcatel-Lucent, *3.3. Initializing an IP Touch 40x8 EE phone* **In** : *IP Touch 4008/4018 Extended Edition - SIP Phone Installation Guide - 8AL90824AAAA ed02*, p.7-8, Août 2010, Disponible sur : https://www.cluster2.hostgator.co.in/files/writeable/uploads/hostgator136107/file/iptouchsipphoneinstallationguide-ed02.pdf.

<a name="ecdsa"></a>**(11)** : SECTIGO Store, *ECDSA vs RSA: Everything You Need to Know*, 9 juin 2020, Disponible sur : https://sectigostore.com/blog/ecdsa-vs-rsa-everything-you-need-to-know/.

<a name="ecdsa_anssi"></a>**(12)** : Agence nationale de la sécurité des systèmes d'information, *Logarithme discret dans les courbes elliptiques définies sur GF(p)* **In** : *Référentiel Général de Sécurité version 2.0 - Annexe B1*, p.19-20, 21 février 2014, Disponible sur : https://www.ssi.gouv.fr/uploads/2014/11/RGS_v-2-0_B1.pdf.