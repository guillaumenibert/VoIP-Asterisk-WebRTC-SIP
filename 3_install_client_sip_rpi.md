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

## 3. Installation and configuration of a SIP client on the Raspberry Pi

<p style="text-align: justify; text-indent: 3em">
The installation and configuration of a SIP client on the Raspberry Pi is necessary to communicate with VoIP. This client will connect to the Asterisk server and depending on the number the client is calling, the server will use the dial plan defined in <b><i>extensions.conf</i></b> to contact the right endpoint (Alcatel phone for example). 
</p>

### Technology choices
<p style="text-align: justify; text-indent: 3em">
    <ul style="text-align: justify;">
        <li>OS: Raspberry Pi OS Buster (arm64), the 64-bit version is only very recent but seems to be promising. Indeed, it is more powerful than the 32 bits version (armhf) <b>(<a style="text-decoration: none;" href="#rpi_benchmark" name="rpi_benchmark_text">9</a>)</b>. This is a significant advantage, especially on a Raspberry Pi 3B+ which will be used in desktop mode. Any improvement in performance is worthwhile. Furthermore, Raspberry Pi OS is a system officially maintained by the Raspberry Pi Foundation.</li>
        <li>SIP client: <i>linphonec</i>,  the command line version of <i>Linphone</i>. It is stable and works perfectly on Raspberry Pi OS. It is open source. Unfortunately, the popular client <i>Jami</i> (formerly <i>GNU Ring</i>), developed by <i>Savoir-faire Linux</i>  does not seem to work well with Raspberry Pi OS.</li>
    </ul>
</p>

### Prerequisites
<p style="text-align: justify; text-indent:  3em;">
Having an up-to-date Raspberry Pi 3B+ running Raspberry Pi OS Buster (64-bit) connected to the local network and the internet, also with SSH access (see appendix A2 of the PDF report for the detailed implementation of this requirement). The Asterisk server must be running.
</p>
<p style="text-align: justify; text-indent:  3em;">
Consider in this section the following information from this machine:
</p>
<div align="center">

<table>
    <thead>
        <tr>
            <th>IP address</th>
            <th style="min-width: 110px">User</th>
            <th>Password</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Ethernet: 192.168.1.82</td>
            <td rowspan=2>pi</td>
            <td rowspan=2>voippiutc</td>
        </tr>
        <tr>
            <td>Wi-Fi: 192.168.1.92</td>
        </tr>
    </tbody>
</table>

</div><br>

### Installation and configuration of the Linphone SIP client

1. Start the Raspberry Pi then launch a terminal and install Linphone.

```bash
sudo apt install linphone -y
```

2. Once the installation is complete, register the associated ***`rpi`*** user on the server.

```bash
linphonec
```


<pre>
<span style="color:green;">#linphonec> register sip:login@domain domain &lt;password&gt;</span>

linphonec> register sip:rpi@192.168.1.80 192.168.1.80 22222222
</pre>
<p style="text-align: justify;">
    The <b><i><code>rpi</code></i></b> client is connected to the Asterisk server. It can therefore contact the Alcatel telephone and vice versa.
</p>

### Client-side testing

<p style="text-align: justify; text-indent:  3em;">
If the registration on the server was successful, the previously executed command returns this:
</p>

```
linphonec> Refreshing on sip:rpi@192.168.1.80…
linphonec> Registration on sip:192.168.1.80 successful.
linphonec>
```

### Server-side testing

<p style="text-align: justify; text-indent:  3em;">
    On a server console, type the command <b><i>sudo asterisk -rvvv</i></b>.
</p>
<p style="text-align: justify; text-indent:  3em;">
    Once entered, type the command <b><i>pjsip show endpoints</i></b>. If the Raspberry Pi's SIP client is connected then the console will return this:
</p>

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

 Endpoint:  alcatel/5001                                 Unavailable   0 of inf
     InAuth:  alcatel/alcatel
        Aor:  alcatel                                        1

 Endpoint:  guillaume/5003                               Unavailable   0 of inf
     InAuth:  guillaume/guillaume
        Aor:  guillaume                                      1

<span style="color:deeppink;"><b> Endpoint:  rpi/5002                                     Not in use    0 of inf
     InAuth:  rpi/rpi
        Aor:  rpi                                            1
      Contact:  rpi/sip:rpi@192.168.1.82;transport=udp  cec2f9dd2f NonQual  nan</b></span>


Objects found: 3
</pre>

<p style="text-align: justify; text-indent:  3em;">
    It is clear that the client has an IP address of <b><i>192.168.1.82</i></b> and is connected. The information <b><i>"Not in use"</i></b> indicates that there is no call in progress.
</p>
<p style="text-align: justify; text-indent:  3em;">
The Alcatel phone is still not connected, it is time to integrate it into the system.
</p>

## [4. IP phone configuration](4_config_alcatel.md)

## [5. Communication tests](5_tests_com_sip.md)

## [6. JavaScript SIP client using WebRTC](6_sip_webrtc.md)

## [Conclusion](Conclusion.md)

## [Abbreviations](Abbreviations.md)

## Reference on this page

<p><a name="rpi_benchmark"></a><strong>(<a style="text-decoration: none;" href="#rpi_benchmark_text">9</a>)</strong>: bluesman, <em>64 bit Raspberry Pi OS is here!</em>, Audiophile Style, 4<sup>th</sup> june 2020, available at: <a href="https://audiophilestyle.com/forums/topic/59499-64-bit-raspberry-pi-os-is-here/" hreflang="en" target="_blank">https://audiophilestyle.com/forums/topic/59499-64-bit-raspberry-pi-os-is-here/</a>.</p>