<div align="center">
<br>
<img src="https://www.utc.fr/wp-content/uploads/sites/28/2019/05/SU-UTC18-70.svg" alt="Université de Technologie de Compiègne" width="400">
<br>
<br>

# AI14 - TZ - Mise en place d'une communication VoIP entre un Raspberry Pi et un téléphone IP


**Guillaume Nibert  
Encadrant : Dr. Ahmed Lounis**

</div>

## [Contexte](README.md)

## [1. Protocole SIP et communication VoIP](1_sip_voip.md)

## [2. Mise en place d'un serveur PABX IP Asterisk](2_ipbx_asterisk.md)

## 3. Installation et configuration d'un client SIP sur le Raspberry Pi

L’installation et la configuration d’un client SIP sur le Raspberry Pi est nécessaire pour communiquer en VoIP. Ce client va se connecter au serveur Asterisk et en fonction du numéro que le client appelle, le serveur se sert du plan de numérotation défini dans ***`extensions.conf`*** pour contacter le bon point de terminaison (téléphone Alcatel par exemple).

### Choix technologiques
- OS : Raspberry Pi OS Buster (arm64), la version 64 bits n’est que très récente mais semble être prometteuse. En effet, elle est plus performante que la version 32 bits (armhf) **([9](#rpi_benchmark))**. C’est un avantage important non négligeable surtout sur une Raspberry Pi 3B+ qui va être utilisée dans un mode Desktop. Toute amélioration des performances est intéressante. Par ailleurs, Raspberry Pi OS est un système officiellement maintenu par la fondation Raspberry Pi.
- Client SIP : *linphonec*, la version en ligne de commande de *Linphone*. Elle est stable et fonctionne parfaitement sur Raspberry Pi OS. Elle est open source. Malheureusement, le client populaire *Jami* (anciennement *GNU Ring*), développé par *Savoir-faire Linux* semble ne pas fonctionner correctement avec Raspberry Pi OS.

### Prérequis
Disposer d’un Raspberry Pi 3B+ sous Raspberry Pi OS Buster (64 bits) à jour connecté au réseau local et à internet, disposant également d’un accès SSH (cf. annexe A2 du rapport PDF pour la mise en place détaillée de ce prérequis). Le serveur Asterisk doit être en fonctionnement.

Considérons dans cette partie les informations suivantes de cette machine :

<div align="center">

<table>
    <thead>
        <tr>
            <th>Adresse IP</th>
            <th>Utilisateur</th>
            <th>Mot de passe</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Ethernet : 192.168.1.82</td>
            <td rowspan=2>pi</td>
            <td rowspan=2>voippiutc</td>
        </tr>
        <tr>
            <td>Wi-Fi : 192.168.1.92</td>
        </tr>
    </tbody>
</table>

</div>

### Installation et configuration du client SIP Linphone

1. Démarrer le Raspberry Pi puis lancer un terminal et installer Linphone.

```bash
sudo apt install linphone -y
```

2. Une fois l’installation terminée, enregistrer l’utilisateur ***`rpi`*** associé sur le serveur.

```bash
linphonec
```

*`linphonec>`*

```bash
#register sip:identifiant@domaine domaine <mot_de_passe>

register sip:rpi@192.168.1.80 192.168.1.80 22222222
```

Le client ***`rpi`*** est bien connecté au serveur Asterisk. Il peut donc contacter le téléphone Alcatel et réciproquement.

### Vérification côté client

Si l’enregistrement sur le serveur a réussi, la commande exécutée précédemment renvoie ceci :

```
linphonec> Refreshing on sip:rpi@192.168.1.80…
linphonec> Registration on sip:192.168.1.80 successful.
linphonec>
```

### Vérification côté serveur

Sur une console serveur, taper la commande `sudo asterisk -rvvv`.

Une fois entrée, taper la commande `pjsip show endpoints`. Si le client SIP du Raspberry Pi est bien connecté alors la console renvoie ceci :

*Output*

```
asterisktz*CLI> pjsip show endpoints

 Endpoint:  <Endpoint/CID.....................................>  <State.....>  <Channels.>
    I/OAuth:  <AuthId/UserName...........................................................>
        Aor:  <Aor............................................>  <MaxContact>
      Contact:  <Aor/ContactUri..........................> <Hash....> <Status> <RTT(ms)..>
  Transport:  <TransportId........>  <Type>  <cos>  <tos>  <BindAddress..................>
   Identify:  <Identify/Endpoint.........................................................>
        Match:  <criteria.........................>
    Channel:  <ChannelId......................................>  <State.....>  <Time.....>
        Exten: <DialedExten...........>  CLCID: <ConnectedLineCID.......>
==========================================================================================

 Endpoint:  alcatel/5001                                         Unavailable   0 of inf
     InAuth:  alcatel/alcatel
        Aor:  alcatel                                            1

 Endpoint:  guillaume/5003                                       Unavailable   0 of inf
     InAuth:  guillaume/guillaume
        Aor:  guillaume                                          1

 Endpoint:  rpi/5002                                             Not in use    0 of inf
     InAuth:  rpi/rpi
        Aor:  rpi                                                1
      Contact:  rpi/sip:rpi@192.168.1.82;transport=udp     cec2f9dd2f NonQual     nan


Objects found: 3
```

On remarque bien que le client a pour adresse IP ***192.168.1.82*** et qu’il est bien connecté. Le ***Not in use*** indique qu’il n’y a pas d’appel en cours.

Le téléphone Alcatel n’est en revanche toujours pas connecté. Nous allons donc le faire en partie suivante.

## [4. Configuration du téléphone IP](4_config_alcatel.md)

## [5. Tests de communication](5_tests_com_sip.md)

## [6. Client SIP JavaScript utilisant WebRTC](6_sip_webrtc.md)

## [Conclusion](Conclusion.md)

## [Sigles](Sigles.md)

## Référence de cette page

<a name="rpi_benchmark"></a>**(9)** : bluesman, *64 bit Raspberry Pi OS is here!*, Audiophile Style, 4 juin 2020, Disponible sur : https://audiophilestyle.com/forums/topic/59499-64-bit-raspberry-pi-os-is-here/.