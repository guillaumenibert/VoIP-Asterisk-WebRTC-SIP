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

## [3. Installation et configuration d'un client SIP sur le Raspberry Pi](3_install_client_sip_rpi)

## [4. Configuration du téléphone IP](4_config_alcatel.md)

## 5. Tests de communication

Tous les points de terminaison sont connectés au serveur Asterisk. Il est donc possible d’appeler du Raspberry Pi vers l’Alcatel ou de l’Alcatel vers le Raspberry Pi.

Afin de réaliser la communication, il faut préalablement avoir lancé le serveur Asterisk, le serveur TFTP, allumé tous les périphériques et branché une sortie audio sur le Raspberry Pi (jack, Bluetooth ou HDMI) pour pouvoir écouter le flux audio.

### Raspberry Pi vers Alcatel IP Touch

1. Lancer un terminal puis exécuter ***linphonec***.

```bash
linphonec
```

2. Appeler le téléphone Alcatel IP Touch, il a pour numéro **`5001`** (cf. **[partie 2 - Configuration de SIP et création d’utilisateurs](2_ipbx_asterisk.md)**).

```
linphonec> call 5001
```

*Output (commenté)*

```bash
# Message d’erreur non important, la vidéo est bien désactivée on ne fait que de la VoIP.
2021-02-09 13:30:36:367 ortp-error-LinphoneCore has video disabled for both capture and display, but video policy is to start the call with video. This is a possible mis-use of the API. In this case, video is disabled in default LinphoneCallParams

# Établissement de lien vers l’Alcatel
Establishing call id to sip:5001@192.168.1.80, assigned id 1
# L’Alcatel a été trouvé, il est contacté, ça sonne du côté de l’Alcatel.
Contacting sip:5001@192.168.1.80
linphonec> Call 1 to sip:5001@192.168.1.80 in progress.
linphonec> Call 1 with sip:5001@192.168.1.80 connected.
# Nous avons décroché le téléphone Alcatel.
Call answered by sip:5001@192.168.1.80
# La communication est en cours, l’audio passe, les paramètres sont ajustés.
linphonec> 2021-02-09 13:30:36:563 ortp-error-no such method on filter MSPulseWrite, fid=16394 method index=2
Media streams established with sip:5001@192.168.1.80 for call 1 (audio).
Call is updated by remote.
linphonec> 2021-02-09 13:30:40:761 ortp-error-no such method on filter MSPulseWrite, fid=16394 method index=2
Call parameters were successfully modified.
linphonec> Media streams established with sip:5001@192.168.1.80 for call 1 (audio).
Call is updated by remote.
linphonec> Call parameters were successfully modified.
linphonec> Media streams established with sip:5001@192.168.1.80 for call 1 (audio).
# La communication vient de se terminer, quelqu’un a raccroché un des appareils.
Call terminated.
linphonec> Call 1 with sip:5001@192.168.1.80 ended (No error).
```

Au moment où l’appel est lancé, voici ce que l’écran du téléphone Alcatel affiche :

<div align="center">
<img src="figures/figure15_sip_rpi_alcatel.png" alt="Figure 15 - Appel de Raspberry Pi vers Alcatel IP Touch 4018 EE">

*(Figure 15 - Appel de Raspberry Pi vers Alcatel IP Touch 4018 EE)*

</div>

La communication fonctionne donc dans un sens. Voyons ce que cela donne si c’est l’Alcatel qui appelle le Raspberry Pi.

### Alcatel IP Touch vers Raspberry Pi

1. Depuis le téléphone, appeler le **`5002`**, numéro du Raspberry Pi (cf. **[partie 2 - Configuration de SIP et création d’utilisateurs](2_ipbx_asterisk.md)**).

<div align="center">
<img src="figures/figure16_sip_alcatel_rpi.png" alt="Figure 16 - Appel de Alcatel IP Touch 4018 EE vers Raspberry Pi">

*(Figure 16 - Appel de Alcatel IP Touch 4018 EE vers Raspberry Pi)*

</div>

2. Depuis le terminal Raspberry Pi, veiller à ce que ***linphonec*** soit actif. Au moment où l’Alcatel lance son appel, on le reçoit dans le terminal :

*Output*

```
linphonec> Receiving new incoming call from "Alcaltel IP Touch" <sip:5001@192.168.1.80>, assigned id 3
```

Pour y répondre, il suffit de taper ***`answer`*** et l’***id*** de l’appel :

```
answer 3
```

La communication est lancée et fonctionne de la même manière.

*Output*

```
linphonec> Receiving new incoming call from "Alcaltel IP Touch" <sip:5001@192.168.1.80>, assigned id 3
answer 3
Connected.
linphonec> Call 3 with "Alcaltel IP Touch" <sip:5001@192.168.1.80> connected.
2021-02-09 13:46:28:345 ortp-error-no such method on filter MSPulseWrite, fid=16394 method index=2
Media streams established with "Alcaltel IP Touch" <sip:5001@192.168.1.80> for call 3 (audio).
linphonec> Call is updated by remote.
linphonec> 2021-02-09 13:46:28:424 ortp-error-no such method on filter MSPulseWrite, fid=16394 method index=2
Call parameters were successfully modified.
linphonec> Media streams established with "Alcaltel IP Touch" <sip:5001@192.168.1.80> for call 3 (audio).
Call is updated by remote.
linphonec> Call parameters were successfully modified.
linphonec> Media streams established with "Alcaltel IP Touch" <sip:5001@192.168.1.80> for call 3 (audio).
Call terminated.
linphonec> Call 3 with "Alcaltel IP Touch" <sip:5001@192.168.1.80> ended (No error).
```

Les communications fonctionnent donc dans les deux sens. L’objectif de la partie suivante est donc de réaliser une interface graphique en JavaScript côté Raspberry Pi, plus conviviale que le client ***linphonec*** en ligne de commande.

## [6. Client SIP JavaScript utilisant WebRTC](6_sip_webrtc.md)

## [Conclusion](Conclusion.md)

## [Sigles](Sigles.md)