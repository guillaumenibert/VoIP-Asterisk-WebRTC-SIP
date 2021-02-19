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

## 2. Mise en place d'un serveur PABX IP Asterisk

Dans les grandes structures (entreprises, services publics...), on retrouve principalement deux types de communications téléphoniques : la communication interne et la communication externe. Les agents se retrouvent donc avec un téléphone fixe interne capable d'émettre des appels vers un autre téléphone fixe interne ou un téléphone externe (public). Pour que cela fonctionne, il faut un PABX, aussi appelé autocommutateur téléphonique privé.

Le PABX (ci-dessous), présente de nombreux avantages, notamment :
 - financièrement, la facture s’allège, les appels internes ne passent pas par le réseau public (Orange, anciennement France Télécom) ;
 - on peut attribuer davantage de numéros interne sans difficulté ;
 - il est possible de proposer des services tels que la conférence téléphonique, les renvois, le transfert d’appel ;
 - et, bien entendu, relier une ligne interne à une ligne externe.

<div align="center">
<img src="figures/figure05_pabx_matra_mc6500.png" alt="Figure 5 - PABX Matra série MC6500" height=250>

*(Figure 5 - PABX Matra série MC6500)*

</div>

Vous trouverez une liste exhaustive des fonctions d’un PABX traditionnel sur Wikipédia **([1](#wikipedia_pabx))**.

Le fonctionnement global de ce système étant présenté, comment peut-on donc réaliser une communication utilisant le protocole SIP ?

Historiquement, il y a eu trois grandes phases dans l’évolution des communications téléphoniques :
1. Le réseau téléphonique commuté (RTC), analogique. Dans ce réseau, impossible d’utiliser le protocole SIP puisque ce dernier est numérique.
2. Le réseau numérique à intégration de services (RNIS ou ISDN dans sa version anglaise), numérique, dont la base de la pile réseau est conforme au modèle OSI. À partir de la couche 3 (transport), on retrouve les protocoles suivants : Q.931 **([2](#q931))**, X.25 couche 3 **([3](#x253))**. Ici, c’est la couche 3 qui pose problème. SIP s’appuie sur IP pour fonctionner.
3. La voix sur IP, sur le réseau Internet, est numérique. SIP pourra fonctionner dans ce cas. Le PABX associé est un PABX IP ou encore appelé IPBX (Internet Protocol Branch eXchange).

Il existe de nombreux IPBX dans le monde. Asterisk a la particularité d’être le numéro 1 mondial en termes d’utilisation. Il comporte une version libre et une version propriétaire. Et propose une interopérabilité avec les anciens réseaux (RTC et RNIS) au moyen de cartes matérielles et de modules logiciels. Ce dernier point est probablement un grand facteur de son succès : les entreprises qui utilisent encore du vieux matériel, et qui, dans une optique de modernisation migrent vers du matériel récent, utilisent probablement plusieurs systèmes (RTC, RNIS ou encore VoIP), Asterisk va permettre de gérer simultanément ces trois systèmes.

Procédons donc à la mise en place d’Asterisk.

### Choix technologiques

- OS : Debian 10, il est libre et est principalement utilisé en tant que serveur dans le monde informatique. C’est un système proposant des paquets régulièrement mis à jour en termes de stabilité et de sécurité.
- Asterisk : version 18 LTS compilée à partir de ses sources. La version dans les dépôts de Debian est ancienne (16 pour Debian 10) et bien qu'elle soit aussi une version LTS, le fait qu'elle soit déjà compilée offre moins de flexibilité en termes d’ajout de modules. Par ailleurs, le module assurant la gestion du protocole SIP dans la version 16 est dépréciée depuis la version 17 **([4](#sip_deprecie))** au profit d’un nouveau module ([PJSIP](https://www.pjsip.org/)) capable de gérer le protocole SIP ainsi que les fonctions de traversée de NAT avec SIP **([5](#problematique_traversee_nat))**.


## Prérequis

Avoir une machine Debian 10 (sans interface graphique) et à jour connectée au réseau local et à internet, disposant également d’un accès SSH (cf. annexe A1 du rapport PDF pour la mise en place détaillée de ce prérequis).

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
            <td rowspan=2>192.168.1.80</td>
            <td>asterisktz</td>
            <td>voiputc</td>
        </tr>
        <tr>
            <td>root</td>
            <td>voiputc</td>
        </tr>
    </tbody>
</table>

</div>

## Installation d'Asterisk
*Remarque : nous n’installerons pas les librairies permettant la gestion des cartes d’interface téléphoniques de Digium (société maintenant Asterisk) et la gestion des protocoles utilisés dans les réseaux RNIS.*

1. Se connecter en SSH à la machine `asterisk`.
```bash
# ssh login@adresse_ip_vm -p 22
ssh asterisktz@192.168.1.80 -p 22
```

2. Installer les paquets permettant la compilation d’Asterisk et des prérequis.
```bash
sudo apt update && sudo apt install linux-headers-$(uname -r) build-essential autoconf libglib2.0-dev libtool net-tools
```

3. Redémarrer la machine Debian et se connecter à nouveau en SSH.
```bash
sudo reboot
```
```bash
# Reconnexion en SSH
ssh asterisktz@192.168.1.80 -p 22
```

4. Se placer dans le répertoire `/usr/src` et télécharger Asterisk 18 à cette adresse : https://downloads.asterisk.org/pub/telephony/asterisk/asterisk-18-current.tar.gz. 

```bash
cd /usr/src
sudo wget https://downloads.asterisk.org/pub/telephony/asterisk/asterisk-18-current.tar.gz
```

5. Décompresser l'archive.
```bash
sudo tar -zxvf asterisk-18-current.tar.gz
```

6. Exécuter le script install_prereq installant les prérequis. *Remarque : pour connaître le numéro de version mineure, taper `ls`. Dans ce rapport, il s’agit d’Asterisk 18.2.*

```bash
cd asterisk-18.2.0/contrib/scripts/
sudo ./install_prereq install
```

Pendant l’installation des prérequis une fenêtre s’affiche demandant de sélectionner l’indicatif des numéros de téléphones. Sélectionner ***33*** pour la France et valider par `<Ok>`.

<div align="center">
<img src="figures/figure06_indicatif.png" alt="Figure 6 - Paramétrage indicatif téléphonique">

*(Figure 6 - Paramétrage indicatif téléphonique)*

</div>

7. Vérification des dépendances requises.

```bash
cd ../..
sudo ./configure
```

Si l’opération se déroule bien, on obtient un message similaire à celui-ci avec le logo d’Asterisk :

```
                .$$$$$$$$$$$$$$$=..      
              .$7$7..        .7$$7:.    
            .$7$7..           .7$$7:.
          .$$:.                 ,$7.7
        .$7.     7$$$$           .$$77
     ..$$.       $$$$$            .$$$7
    ..7$   .?.   $$$$$   .?.       7$$$.
   $.$.   .$$$7. $$$$7 .7$$$.      .$$$.
 .777.   .$$$$$$77$$$77$$$$$7.      $$$,
 $$$~      .7$$$$$$$$$$$$$7.       .$$$.
.$$7          .7$$$$$$$7:          ?$$$.
$$$          ?7$$$$$$$$$$I        .$$$7
$$$       .7$$$$$$$$$$$$$$$$      :$$$.
$$$       $$$$$$7$$$$$$$$$$$$    .$$$.
$$$        $$$   7$$$7  .$$$    .$$$.
$$$$             $$$$7         .$$$.
7$$$7            7$$$$        7$$$
 $$$$$                        $$$
  $$$$7.                       $$  (TM)
   $$$$$$$.           .7$$$$$$  $$
     $$$$$$$$$$$$7$$$$$$$$$.$$$$$$
       $$$$$$$$$$$$$$$$.

configure: Package configured for:
configure: OS type : linux-gnu
configure: Host CPU : x86_64
configure: build-cpu:vendor:os: x86_64 : pc : linux-gnu :
configure: host-cpu:vendor:os: x86_64 : pc : linux-gnu :
```

Si cela s’est mal déroulé, voici un lien vers la documentation : https://wiki.asterisk.org/wiki/display/AST/Checking+Asterisk+Requirements.

8. Sélection des options à installer pour Asterisk. Veiller à ce que le terminal ait une taille minimale de `80 x 27`.

```bash
sudo make menuselect
```

Une fenêtre s’affiche permettant de choisir les options.

Aller dans la section Core Sound Package pour installer les sons français (messages vocaux) :
 - Déselectionner le package `CORE-SOUNDS-EN-GSM`. Il s’agit des sons anglais avec le codec audio GSM.
 - Sélectionner les packages `CORE-SOUNDS-FR-ULAW` et `CORE-SOUNDS-FR-ALAW`. Il s’agit des sons français avec les codecs ULAW et ALAW (pris en charge par l’Alcatel IP Touch 4018EE **([6](#alaw_ulaw_alcatel))**). Ces codecs sont définis par la norme G.711 de l’UIT : https://www.itu.int/rec/T-REC-G.711-198811-I/fr. Le codec ALAW est utilisé en Europe et en Afrique. Le codec ULAW en Amérique du Nord et au Japon **([7](#alaw_ulaw_geo))**.

<div align="center">
<img src="figures/figure07_core_sound.png" alt="Figure 7 - Sélection des modules de sons d’Asterisk">

*(Figure 7 - Sélection des modules de sons d’Asterisk)*

</div>

Puis dans la section Extra Sound Packages, sélectionner les packages `EXTRA-SOUNDS-FR-ULAW` et `EXTRA-SOUNDS-FR-ALAW`.

<div align="center">
<img src="figures/figure08_extras_sound.png" alt="Figure 8 - Sélection des modules de sons extra d’Asterisk">

*(Figure 8 - Sélection des modules de sons extra d’Asterisk)*

</div>

Valider en tapant la touche `F12`.

9. Compiler le programme avec les options précédemment choisies puis installer Asterisk.

```bash
sudo make
```

La compilation prend du temps (en général ~15 min) selon la puissance de la machine. Une fois réussie...

```
+--------- Asterisk Build Complete ---------+
+ Asterisk has successfully been built, and +
+ can be installed by running:              +
+                                           +
+                make install               +
+-------------------------------------------+
+--------- Asterisk Build Complete ---------+
```

...on installe Asterisk.

```bash
sudo make install
```

L’installation se termine sur ce message :

```
 +---- Asterisk Installation Complete -------+
 +                                           +
 +    YOU MUST READ THE SECURITY DOCUMENT    +
 +                                           +
 + Asterisk has successfully been installed. +
 + If you would like to install the sample   +
 + configuration files (overwriting any      +
 + existing config files), run:              +
 +                                           +
 + For generic reference documentation:      +
 +    make samples                           +
 +                                           +
 + For a sample basic PBX:                   +
 +    make basic-pbx                         +
 +                                           +
 +                                           +
 +-----------------  or ---------------------+
 +                                           +
 + You can go ahead and install the asterisk +
 + program documentation now or later run:   +
 +                                           +
 +               make progdocs               +
 +                                           +
 + **Note** This requires that you have      +
 + doxygen installed on your local system    +
 +-------------------------------------------+
```

10. Créer les fichiers d’exemples de configuration dans le dossier `/etc/asterisk`.

```bash
sudo make samples
```

11. Installer les scripts de démarrage.

```bash
sudo make config
sudo ldconfig
```

12. Démarrage automatique du service Asterisk au lancement de la machine.

```bash
cd
sudo groupadd asterisk
sudo useradd -r -d /var/lib/asterisk -g asterisk asterisk
sudo usermod -aG audio,dialout asterisk
sudo chown -R asterisk.asterisk /etc/asterisk
sudo chown -R asterisk.asterisk /var/{lib,log,spool}/asterisk
sudo chown -R asterisk.asterisk /usr/lib/asterisk
```

```
sudo nano /etc/default/asterisk
```

Décommenter les lignes `AST_USER="asterisk"` et `AST_GROUP="asterisk"` (enlever le croisillon `#` devant chaque ligne). Enregistrer avec ***Ctrl + O***. Quitter avec ***Ctrl + X***.

<div align="center">
<img src="figures/figure09_asterisk_default_user.png" alt="Figure 9 - Configuration d’asterisk en temps qu’utilisateur par défaut du service Asterisk">

*(Figure 9 - Configuration d’`asterisk` en temps qu’utilisateur par défaut du service Asterisk)*

</div>

```bash
sudo nano /etc/asterisk/asterisk.conf
```

Décommenter les lignes (enlever le point-virgule `;` devant chaque ligne). Enregistrer avec ***Ctrl + O***. Quitter avec ***Ctrl + X***.

```
runuser = asterisk ; The user to run as.
rungroup = asterisk ; The group to run as.
```

<div align="center">
<img src="figures/figure10_asterisk_default_user.png" alt="Figure 10 - Configuration d’asterisk en temps qu’utilisateur par défaut du service Asterisk">

*(Figure 10 - Configuration d’`asterisk` en temps qu’utilisateur par défaut du service Asterisk)*

</div>

13. Démarrer le service Asterisk

```bash
sudo systemctl start asterisk
```

Vous pouvez maintenant vérifier son statut actuel via la commande suivante :

```bash
sudo systemctl status asterisk
```

<div align="center">
<img src="figures/figure11_asterisk_errors.png" alt="Figure 10 - Configuration d’asterisk en temps qu’utilisateur par défaut du service Asterisk">

*(Figure 11 - Lancement du service Asterisk avec erreurs)*

</div>

À ce stade, s’il y a ces deux lignes rouges, on peut les corriger de cette manière (fix provenant de https://clearhat.org/blog/a-fix-for-apt-install-asterisk-on-ubuntu-18-04) :

```bash
sudo systemctl stop asterisk

sudo sed -i 's";\[radius\]"\[radius\]"g' /etc/asterisk/cdr.conf

sudo sed -i 's";radiuscfg => /usr/local/etc/radiusclient-ng/radiusclient.conf"radiuscfg => /etc/radcli/radiusclient.conf"g' /etc/asterisk/cdr.conf

sudo sed -i 's";radiuscfg => /usr/local/etc/radiusclient-ng/radiusclient.conf"radiuscfg => /etc/radcli/radiusclient.conf"g' /etc/asterisk/cel.conf

sudo systemctl start asterisk
```

<div align="center">
<img src="figures/figure12_asterisk_without_errors.png" alt="Figure 12 - Lancement du service Asterisk sans erreur">

*(Figure 12 - Lancement du service Asterisk sans erreur)*

</div>

14. Démarrage automatique du service Asterisk.

```bash
sudo /lib/systemd/systemd-sysv-install enable asterisk
```

Asterisk est opérationnel. Passons donc à la création d’utilisateurs et la configuration SIP !

### Configuration de SIP et création d'utilisateurs

Nous avons donc un serveur opérationnel, il faut donc maintenant configurer SIP et créer des utilisateurs.
Nous allons créer 3 utilisateurs dont les caractéristiques sont les suivantes :

<div align="center">

|                     | Téléphone Alcatel                                       | Raspberry Pi                      | Test                        |
| ------------------- | ------------------------------------------------------- | --------------------------------- | --------------------------- |
| Usage               | Compte dédié pour le téléphone Alcatel IP Touch 4018 EE | Compte dédié pour le Raspberry Pi | Compte dédié pour les tests |
| Nom d’affichage     | Alcatel IP Touch                                        | Raspberry Pi                      | Guillaume Nibert            |
| Numéro de téléphone | 5001                                                    | 5002                              | 5003                        |
| Identifiant         | alcaltel                                                | rpi                               | guillaume                   |
| Mot de passe        | 11111111                                                | 22222222                          | 33333333                    |

</div>

Il y a deux fichiers de configuration à éditer : `pjsip.conf` et `extensions.conf`. Le premier sert à créer les comptes, configurer le fonctionnement de SIP (UDP/TCP), les systèmes d’authentification… et le deuxième à définir le comportement du système, plus précisément le plan de numérotation (similaire au routage si l’on parlait de paquets IP). Ce “routage” se fait en fonction des numéros de téléphone (identifiants).
Ces fichiers sont présents dans `/etc/asterisk` et dans le répertoire `asterisk_sip` du dépôt Git.

### Configuration de SIP - `pjsip.conf`

1. Renommer le fichier de configuration `pjsip.conf` par `pjsip_original.conf`.

```bash
sudo mv /etc/asterisk/pjsip.conf /etc/asterisk/pjsip_original.conf
```

2. Créer un fichier `pjsip.conf`...

```bash
sudo nano /etc/asterisk/pjsip.conf
```

...et placer le contenu suivant :

```conf
[transport-udp]
type=transport
protocol=udp
bind=0.0.0.0
 
;Modèles de base, ils seront recopiés pour chaque utilisateur
 
[endpoint_basic](!)
type=endpoint         ; point de terminaison (téléphone/raspberry/pc...)
context=plan-num      ; se sert du plan de numérotation défini dans extensions.conf
disallow=all          ; désactivation de tous les codecs audio
allow=ulaw            ; sauf le codec ULAW
allow=alaw            ; et le codec ALAW
language=fr
 
[authentication](!)
type=auth             ; type de la section : authentification
auth_type=userpass    ; authentification par mot de passe
 
[aor_template](!)
type=aor              ; savoir où le point de terminaison peut être contacté
max_contacts=1
 
;Définitions des comptes utilisateurs associés aux matériels
 
[alcatel](endpoint_basic)
auth=alcatel
aors=alcatel
callerid="Alcaltel IP Touch" <5001> ; pour avoir le nom de l'appelant qui s'affiche
[alcatel](authentication)
password=11111111
username=alcatel
[alcatel](aor_template)
 
[rpi](endpoint_basic)
auth=rpi
aors=rpi
callerid="Raspberry Pi" <5002>
[rpi](authentication)
password=22222222
username=rpi
[rpi](aor_template)
 
[guillaume](endpoint_basic)
auth=guillaume
aors=guillaume
callerid="Guillaume Nibert" <5003>
[guillaume](authentication)
password=33333333
username=guillaume
[guillaume](aor_template)
```

La création des comptes et la configuration de SIP est terminée. Passons au plan de numérotation.

### Plan de numérotation - `extensions.conf`

1. Renommer le fichier de configuration `extensions.conf` par `extensions_original.conf`.

```bash
sudo mv /etc/asterisk/extensions.conf /etc/asterisk/extensions_original.conf
```

2. Créer un fichier `extensions.conf`...

```bash
sudo nano /etc/asterisk/extensions.conf
```

...et placer le contenu suivant :

```conf
[plan-num]
exten => 5001,1,Answer(500)
exten => 5001,2,Dial(PJSIP/alcatel,25)
exten => 5001,3,Hangup()
 
exten => 5002,1,Answer(500)
exten => 5002,2,Dial(PJSIP/rpi,25)
exten => 5002,3,Hangup()
 
exten => 5003,1,Answer(500)
exten => 5003,2,Dial(PJSIP/guillaume,25)
exten => 5003,3,Hangup()
 
; les 1, 2 et 3 correspondent aux priorités des appels des
; applications Answer(), Dial() et Hangup(). 1 étant la priorité la
; plus forte. On peut également écrire 1,n,n où le premier n
; correspond à 2 et le deuxième à 3.
```

“L'application ***Answer()*** prend un délai (en millisecondes) comme premier paramètre. L'ajout d'un court délai est souvent utile pour s'assurer que le point de terminaison ait le temps de commencer à traiter l'audio avant de commencer la communication via l’application ***Dial()***. Sinon, vous risquez de ne pas entendre le tout début” **([8](#extensions_conf))**. ***Hangup()*** comme son nom l’indique raccroche l’appel en cours.

Le plan de numérotation est terminé. Les comptes SIP ont été créés. On peut donc passer à la configuration d’un client SIP sur le Raspberry Pi.



## [3. Installation et configuration d'un client SIP sur le Raspberry Pi](3_install_client_sip_rpi.md)

## [4. Configuration du téléphone IP](4_config_alcatel.md)

## [5. Tests de communication](5_tests_com_sip.md)

## [6. Client SIP JavaScript utilisant WebRTC](6_sip_webrtc.md)

## [Conclusion](Conclusion.md)

## [Sigles](Sigles.md)

## Références de cette page

<a name="wikipedia_pabx"></a>**(1)** : Wikipédia, *Généralités* **In** : *Autocommutateur téléphonique privé*, 26 novembre 2020, Disponible sur : https://fr.wikipedia.org/wiki/Autocommutateur_t%C3%A9l%C3%A9phonique_priv%C3%A9.

<a name="q931"></a>**(2)** : Secteur de la normalisation des télécommunications de l'UIT, *Spécification de la couche 3 de l'interface utilisateur-réseau RNIS pour la commande de l’appel de base*, UIT, Mai 1998, Disponible sur : https://www.itu.int/rec/T-REC-Q.931-199805-I/fr.

<a name="x253"></a>**(3)** : Secteur de la normalisation des télécommunications de l'UIT, *Interface entre équipement terminal de traitement de données et équipement de terminaison de circuit de données pour terminaux fonctionnant en mode paquet et raccordés par circuit spécialisé à des réseaux publics pour données*, UIT, Octobre 1996, Disponible sur : https://www.itu.int/rec/T-REC-X.25-199610-I/fr.

<a name="sip_deprecie"></a>**(4)** : Matt Fredrickson, *PSA: chan_sip status changed to “deprecated” & Asterisk 17.0.0-rc2 Release*, Asterisk.org, 25 septembre 2019, Disponible sur : https://www.asterisk.org/deprecating-chan_sip-asterisk-17-0-0-rc2-release/.

<a name="problematique_traversee_nat"></a>**(5)** : Odin Gremaud, *Introduction* **In** : *La traversée de NAT en VoIP SIP*, NEXCOM Systems, p.1, 20 juin 2012, Disponible sur : https://www.nexcom.fr/wp-content/uploads/whitepapers/bcp_nat_traversal.pdf.

<a name="alaw_ulaw_alcatel"></a>**(6)** : Alcaltel-Lucent, *Caractéristiques audio* **In** : *Téléphones Alcatel-Lucent IP Touch 4008/4018 Extended Edition*, p.2, 2010, Disponible sur : https://www.bureautique-communication.fr/fileuploader/download/download/?d=0&file=custom%2Fupload%2F1%2F6%2F16590.pdf.

<a name="alaw_ulaw_geo"></a>**(7)** : Wikipédia,, *G.711*, 17 juillet 2017, Disponible sur : https://fr.wikipedia.org/wiki/G.711.

<a name="extensions_conf"></a>**(8)** : Malcolm Davenport, *Answer, Playback, and Hangup Applications* **In** : *Asterisk Documentation*, Asterisk.org, 19 décembre 2013, Disponible sur : https://wiki.asterisk.org/wiki/display/AST/Answer%2C+Playback%2C+and+Hangup+Applications.