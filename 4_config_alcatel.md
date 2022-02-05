<div align="center">
<br>
<img src="https://www.utc.fr/wp-content/uploads/sites/28/2019/05/SU-UTC18-70.svg" alt="University of Technology of Compiègne" width="400">
<br>
<br>

# Setting up a VoIP communication between a Raspberry Pi and an IP phone using an Asterisk IP PBX server

**Guillaume Nibert  
Supervisor: [Dr. Ahmed Lounis](https://www.hds.utc.fr/~lounisah/dokuwiki/)**

</div>

## [Context](README.md)

## [1. SIP protocol and VoIP communication](1_sip_voip.md)

## [2. Implementation of an Asterisk IP PBX server](2_ipbx_asterisk.md)

## [3. Installation and configuration of a SIP client on the Raspberry Pi](3_install_client_sip_rpi)

## 4. IP phone configuration

<p style="text-align: justify; text-indent:  3em;">
The Alcatel IP Touch 4018 EE is a telephone with two operating modes:
</p>

1. NOE (New Office Environment) mode: proprietary communication protocol developed by Alcatel/Lucent;
2. SIP mode.

<p style="text-align: justify; text-indent:  3em;">
First, it must be configured in SIP mode. Then, it has the particularity to configure itself automatically by downloading its configuration files and its firmware files on a <i>TFTP</i>, <i>HTTP</i> or <i>HTTPS</i> server at startup.
</p>
<p style="text-align: justify; text-indent:  3em;">
    Among the three protocols, the most secure is <i>HTTPS</i>. We tried to implement it with 2048 bit RSA certificates and <i>cipher suites</i> compatible with older equipment. Unfortunately, being in a local network, the Alcatel phone seems not to accept self-signed certificates <i>(according to this forum, it seems that Alcatel IP Touch phones only trust the certification authority issued by the Alcatel-Lucent company. The root certificate from Alcatel-Lucent seems to be inside the phone: <br><a href "https://www.alcatelunleashed.com/viewtopic.php?t=28822" hreflang="en" target="_blank">https://www.alcatelunleashed.com/viewtopic.php?t=28822</a>. We do not have the means to get a certificate from the Alcatel-Lucent certification authority)</i>. Given this problem, there are only two choices: <i>TFTP</i> or <i>HTTP</i>. These application protocols are not very secure: no authentication, transfer of data in clear text over the network... The choice is therefore made at the level of the transport protocols: <i>TFTP</i> necessarily uses <i>UDP</i>, a non-connected mode, whereas <i>HTTP</i> can be configured to use <i>TCP</i>, a connected mode, which includes error detection and correction. The programs transmitted are firmware, if data is corrupted due to an error, it could brick the phone. So let's use the <i>HTTP</i> protocol associated with the <i>TCP</i> protocol. We will therefore set up a lightweight <i><a href="https://www.nginx.com/" hreflang="en" target="_blank">NGINX</a> HTTP</i> server, whose sole purpose is to provide firmware and configuration files to the Alcatel IP Touch phone. It will be accessible at the address <b><i>192.168.1.80</i></b> on port <b><i>80</i></b>.
</p>
<p style="text-align: justify; text-indent:  3em;">
    Of course, if possible, for production use, we recommend using <i>HTTPS</i>.
</p>

<div align="center">
<img src="figures/figure13_alcatel_http.png" alt="Figure 13 - Retrieving Alcatel phone configuration files via HTTP">

*(Figure 13 - Retrieving Alcatel phone configuration files via HTTP)*

</div>

### Configuration information

<div align="center">

<table>
<thead>
<tr>
<th>IP address of the Alcatel phone. This must be configured on the router (see appendix A1.15 of the PDF report).</th>
<th style="min-width: 110px;">192.168.1.81</th>
</tr>
</thead>
<tbody>
<tr>
<td>IP address and port of the HTTP server</td>
<td>192.168.1.80:80</td>
</tr>
<tr>
<td>PoE Injector connected to the router</td>
<td><a href="https://www.tp-link.com/en/business-networking/accessory/tl-poe150s/" hreflang="en" target="_blank">TP-Link TL-POE150S</a></td>
</tr>
</tbody>
</table>

</div>

### Configuration du téléphone en mode SIP - sur l'appareil

<p style="text-align: justify; text-indent:  3em;">
Connect the phone to the PoE injector and follow the instructions in the installation manual below (screenshots of the procedure).
</p>

<div align="center">
<img src="figures/figure14_alcatel_manual1.png" alt="Figure 14 - SIP mode configuration on the Alcatel IP Touch 4018 EE - 1">
<img src="figures/figure14_alcatel_manual2.png" alt="Figure 14 - SIP mode configuration on the Alcatel IP Touch 4018 EE - 2">

(Figure 14 - Alcatel-Lucent, *3. SIP stand-alone mode* **In**: *IP Touch 4008/4018 Extended Edition - SIP Phone Installation Guide - 8AL90824AAAA ed02*, p.4-5, August 2010, available at: https://www.cluster2.hostgator.co.in/files/writeable/uploads/hostgator136107/file/iptouchsipphoneinstallationguide-ed02.pdf)

</div>

<p style="text-align: justify; text-indent:  3em;">
Once in SIP mode, the phone's IP address must be configured to match the one defined in the router (<b>192.168.1.81</b>), and it must also be provided with the IP address of the <i>HTTP</i> server and its port. 
</p>

1. Connect the phone to the PoE injector.
2. At ***phase 2/5 network setup***, press ***" i "*** then ***" #"*** until the ***" MAC address "*** menu appears.
3. If there is already a password configured, enter ***000000***.
4. Scroll down to ***IP Parameters***, then click ***OK***.
5. Scroll down, then select ***IP mode: Static***.
6. Scroll down, then enter the IP address: ***IP@: 192.168.1.81***
7. Scroll down, then enter the subnet mask: ***Subnet: 255.255.255.0***
8. Scroll down, then enter the router IP address: ***Router: 192.168.1.254***
9. Scroll down, then select from DL Scheme: ***HTTP***.
10. Scroll down, then select Use ***Defaultport***.
11. Scroll down, then select the *HTTP* server address: ***DL Addr: 192.168.1.80***
12. Scroll down, then select the *HTTP* server port: ***DL Port: 80***
13. Scroll down, do not select a *VLAN*.
14. Scroll down to save ***save***, then click ***OK***.

<p style="text-align: justify; text-indent:  3em;">
The configuration on the device is complete, however for now it will keep restarting in a loop as the <i>HTTP</i> server does not yet exist, nor do the associated configuration files and phone firmwares.
</p>

### HTTP server installation

<p style="text-align: justify;">
So let's go back to our Debian virtual machine and install an HTTP server.
</p>

1. Connect via SSH to the `asterisktz` machine.

```bash
ssh asterisktz@192.168.1.80 -p 22
```

2. Install ***nginx*** HTTP server..

```bash
sudo apt install nginx
sudo systemctl status nginx
```
<p style="text-align: justify;">
    If the output shows <b><i>active</i></b> then it is working, otherwise you need to run it <code>sudo systemctl start nginx</code>. Normally it is configured to start automatically at startup, if this is not the case then run the following command: 
</p>

```bash
sudo /lib/systemd/systemd-sysv-install enable nginx
```
<p style="text-align: justify;">
    The <i>HTTP</i> server is ready, all that remains is to transfer the firmware, the information on the SIP account dedicated to the Alcatel phone and the IP address of the Asterisk server.
</p>

### Transfer of configuration files and firmware to the Alcatel phone

<p style="text-align: justify;">
    In practise, there are 4 files to transfer to the HTTP server <b>(<a style="text-decoration: none;" href="#alcatel_conf" name="alcatel_conf_text">10</a>)</b>:
    <ul style="text-align: justify;">
        <li><b><i><code>sipconfig.txt</code></i></b>: global configuration file, contains the settings to be applied to all Alcatel IP Touch 4018EE phones connected to the network.</li>
        <li><b><i><code>sipconfig-MacAddress.txt</code></i></b>: configuration file specific to a single Alcatel IP Touch 4018EE phone. This is the MAC address of the phone in question which must be written in lower case.</li>
        <li><b><i><code>noesip4018</code></i></b>: proprietary firmware containing the SIP protocol application for the Alcatel IP Touch 4018EE.</li>
        <li><b><i><code>datsip4018</code></i></b>: resources containing ringtones and different melodies.</li>
    </ul>
</p>
<p style="text-align: justify; text-indent: 3em;">
As the Alcatel-Lucent website does not necessarily provide the appropriate documentation for the structure of the <b><i>sipconfig.txt</i></b> files, we have based ourselves on an example configuration file created by Florian Duraffourg, a graduate of Télécom SudParis: 
<a href="https://github.com/fduraffourg/utils/blob/master/iptouch/sipconfig-reynoud.txt" hreflang="en" target="_blank">https://github.com/fduraffourg/utils/blob/master/iptouch/sipconfig-reynoud.txt</a>.
</p>
<p style="text-align: justify; text-indent: 3em;">
Concerning the firmwares, also difficult to find on the official Alcatel-Lucent website, we found them on the Alcatel Unleashed forum through fbird's post on March 21st 2016: 
<a href="https://www.alcatelunleashed.com/viewtopic.php?p=95015#p95015" hreflang="en" target="_blank">https://www.alcatelunleashed.com/viewtopic.php?p=95015#p95015</a>.
</p>
<p style="text-align: justify; text-indent: 3em;">
    The most important file is <b><i>sipconfig-MacAddress.txt</i></b> whose important content in bold and green is as follows (we have voluntarily removed the non-essential parts, you will find the complete file in the project repository available in the following paragraph):<br>  
<i><span style="font-size:16px">Note: we added comments in this report, to avoid mistakes, you should take the one located in the repository.</span></i>
</p>

*`sipconfig-MacAddress.txt` (green fields are to be taken into account)*

<pre>
[...]


[dns]

###########################################################################
## The primary DNS IP address HAS TO BE FILLED
## If no DNS, use the SIP proxy address instead
###########################################################################

   <span style="color:green;">dns_addr=192.168.1.254  #  DNS of the Freebox (or another router)</span>
   dns2_addr=
   <span style="color:green;">hostname=192.168.1.254</span>

[sip]

###########################################################################
## Domain name : IP address, FQDN or domain name (see the SIP proxy config)
###########################################################################

   <span style="color:green;">domain_name=192.168.1.80  # Asterisk server's domain</span>

###########################################################################
## Primary SIP proxy and SIP registrar settings
##
## Proxy address : IP address, FQDN or domain name
## Registrar address : IP address, FQDN or domain name (usually, the proxy)
## SIP proxy UDP port : usually 5060
## SIP registrar UDP port : by default 5060
###########################################################################

   <span style="color:green;">proxy_addr=192.168.1.80
   proxy_port=5060
   registrar_addr=192.168.1.80
   registrar_port=5060</span>
   outbound_proxy_addr=
   outbound_proxy_port=

###########################################################################
## Redundancy settings
##
## Proxy address : IP address, FQDN or domain name
## Registrar address : IP address, FQDN or domain name (usually, the proxy)
## SIP proxy UDP port : usually 5060
## SIP registrar UDP port : by default 5060
## sip_transport_mode_survi : Transport mode in PCS mode
##          0 = UDP or TCP
##          1 = UDP
##          2 = TCP
###########################################################################

   <span style="color:green;">proxy2_addr=192.168.1.80
   proxy2_port=5060
   registrar2_addr=192.168.1.80
   registrar2_port=5060</span>
   outbound_proxy2_addr=
   outbound_proxy2_port=
   <span style="color:green;">pcs_addr=192.168.1.80
   pcs_port=5060
   sip_transport_mode_survi=0 # in our case, it will be UDP</span>
   option_timer=120

###########################################################################
## Global SIP parameters
## Transport mode : 0 = UDP or TCP
##                  1 = UDP
##                  2 = TCP
## local_rtp_port : RFC3605 is not supported in this release, so
##                  only default value can be used
## PRACK type : 0 = PRACK supported
##              1 = PRACK required
##              2 = PRACK disabled
## Codec settings : 0 = G711 (PCMU)
##                  4 = G723.1
##                  8 = G711 (PCMA)
##                 18 = G729A
###########################################################################

   register_expire=3600
   register_retry=300
   local_sip_port=
   sip_transport_mode=0 # dans notre cas, ce sera de l’UDP
   local_rtp_port=42000
   local_rtcp_port=42001
   prack_type=0
   preferred_vocoder=8,0,4,18

###########################################################################
## SIP authentication.
##
## Realm : If no authentication, leave empty
## Authentication name : HAS TO BE FILLED
##                       If no authentication, PUT A VALUE LIKE none
## Authentication password : If no authentication, leave empty
###########################################################################

   <span style="color:green;">authentication_realm=192.168.1.80  # authentication is done on the Asterisk server
   authentication_name=alcatel        # username and password
   authentication_password=11111111
   user_name=alcatel
   display_name=Alcatel IP Touch      # display name when calling</span>

[...]

[sntp]

###########################################################################
## SNTP server settings (can be OXE or an external server)
##
## Timezone construction : UT::60:032802:103103  (Paris - 2021)
##          GMT delta : 60 = + sixty minutes from GMT time
##          Daylight saving start (mmddhh) : 032902 = 28 March 2am
##          Daylight saving end (mmddhh) : 103103 = 31 October 3am
## The daylight saving settings HAVE to be changed each year.
###########################################################################

   <span style="color:green;">sntp_addr=192.168.1.254       # To synchronise the phone's time
   timezone=UT::60:032802:103103 # with the Freebox's built-in NTP server
                                 # The time zone is to be changed every year,
                                 # it is set according to the GMT zone and
                                 # manages summer and winter time.</span>

[...]

[init]

###########################################################################
## For IP Touch with SIP binary in 1.xx, 2.00.10 and 2.00.20, equal
## or greater than 2.00.81
##     mode 0 = SIP
##     mode 1 = NOE
##
## For IP Touch with SIP binary 2.00.30 to 2.00.80
##     mode 0 = NOE
##     mode 1 = SIP
###########################################################################

   <span style="color:green;">application_mode=0     # SIP mode (depending on firmware version)</span>

[audio]

###########################################################################
## Tone country : 0 = English
##                1 = French
##                2 = German
##                3 = Italian
##                4 = Spanish
##                5 = Dutch
##                6 = Portuguese
## DTMF type : 0 = RFC2833
##             1 = In-band
##             2 = SIP INFO
## DTMF level / RLR handset / SLR handset / Sidetone handset :
## 0 = 0db, 1 = +3db, 2 = +6db, 3 = -3db, 4 = -6db
## VAD / DTMF feedback / Hearing Aid :
##       0 = VAD not used
##       1 = VAD used
###########################################################################

   <span style="color:green;">tone_country=1    # to be set according to the standards used in France
   dtmf_type=1</span>
   dtmf_level=0
   dtmf_avt_payload_type=96
   vad=0
   dtmf_feedback_enable=0
   rlr_handset=0
   slr_handset=0
   sidetone_handset=2
   hearing_aid_enable=0

[appl]

###########################################################################
## Password to access the administrator menu on the phone (digits only)
## Power priority : 1 = critical
##                  2 = high
##                  3 = low
## Time format : 0 = 24 hours format
##               1 = AM / PM
## Speed dial numbers (first and last name, URI)
###########################################################################

   <span style="color:green;">admin_password=000000   # admin password when pressing i then #.</span>
   bluetooth_parameters=blue
   supported_language=0
   remote_forward_code=
   remote_forward_deactive_code=
   power_priority=
   asset_id=
   time_format=0
   speed_dial_1_first_name=
   speed_dial_1_last_name=
   speed_dial_1_uri=
   speed_dial_2_first_name=
   speed_dial_2_last_name=
   speed_dial_2_uri=
   speed_dial_3_first_name=
   speed_dial_3_last_name=
   speed_dial_3_uri=
   speed_dial_4_first_name=
   speed_dial_4_last_name=
   speed_dial_4_uri=
[...]
    </pre>

<p style="text-align: justify;">
    <ol style="text-align: justify;">
        <li>Download all these 2 firmware files (<b><i><code>noesip4018</code></i></b> and <b><i><code>datsip4018</code></i></b>, found on the Alcatel Unleashed forum) and the 2 configuration files (<b><i><code>sipconfig.txt</code></i></b> and <b><i><code>sipconfig-MacAddress.txt</code></i></b>), available in the project repository: <a href="https://github.com/guillaumenibert/VoIP-Asterisk-WebRTC-SIP/tree/main/iptouch4018ee" hreflang="en" target="_blank">https://github.com/guillaumenibert/VoIP-Asterisk-WebRTC-SIP/tree/main/iptouch4018ee</a>.</li>
        <li>Place them in the <code>/var/www/html</code> of the Debian virtual machine.</li>
        <li>Connect the phone to the PoE injector to turn it on. The Alcatel IP Touch will fetch the latest firmware from the <i>HTTP NGINX</i> server, the configurations and will be connected to the Asterisk server.</li>
    </ol>
</p>

### Client-side testing

<p style="text-align: justify; text-indent: 3em;">
If the phone displays the date and time and does not restart in a loop, it is connected to the Asterisk server. 
</p>

### Server-side testing

<p style="text-align: justify; text-indent:  3em;">
    On a server console, type the command <b><i>sudo asterisk -rvvv</i></b>.
</p>
<p style="text-align: justify; text-indent:  3em;">
    Once entered, type the command <b><i>pjsip show endpoints</i></b>. If the Raspberry Pi's SIP client is connected then the console will return this:
</p>

*Output*

<pre>
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
===============================================================================

<b><span style="color:deeppink;"> Endpoint:  alcatel/5001                                 Not in use    0 of inf
     InAuth:  alcatel/alcatel
        Aor:  alcatel                                        1
      Contact:  alcatel/sip:alcatel@192.168.1.81       8c02c0558c NonQual  nan</span></b>

 Endpoint:  guillaume/5003                               Unavailable   0 of inf
     InAuth:  guillaume/guillaume
        Aor:  guillaume                                      1

<span style="color:deeppink;"> Endpoint:  rpi/5002                                     Not in use    0 of inf
     InAuth:  rpi/rpi
        Aor:  rpi                                            1
      Contact:  rpi/sip:rpi@192.168.1.82;transport=udp cec2f9dd2f NonQual  nan</span>


Objects found: 3
</pre>

<p style="text-align: justify; text-indent:  3em;">
    The client has an IP address of <b><i>192.168.1.81</i></b> and is connected. The information <b><i>"Not in use"</i></b> indicates that there is no call in progress.
</p>

## [5. Communication tests](5_tests_com_sip.md)

## [6. JavaScript SIP client using WebRTC](6_sip_webrtc.md)

## [Conclusion](Conclusion.md)

## [Abbreviations](Abbreviations.md)

## Reference on this page

<p><a name="alcatel_conf"></a><strong>(<a style="text-decoration: none;" href="#alcatel_conf_text">10</a>)</strong>: Alcatel-Lucent, <em>3.3. Initializing an IP Touch 40x8 EE phone</em> <strong>In</strong>: <em>IP Touch 4008/4018 Extended Edition - SIP Phone Installation Guide - 8AL90824AAAA ed02</em>, p.7-8, August 2010, available at: <a href="https://www.cluster2.hostgator.co.in/files/writeable/uploads/hostgator136107/file/iptouchsipphoneinstallationguide-ed02.pdf" hreflang="en" target="_blank">https://www.cluster2.hostgator.co.in/files/writeable/uploads/hostgator136107/file/iptouchsipphoneinstallationguide-ed02.pdf</a>.</p>