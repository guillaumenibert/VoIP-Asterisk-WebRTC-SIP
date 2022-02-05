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

## [4. IP phone configuration](4_config_alcatel.md)

## [5. Communication tests](5_tests_com_sip.md)

## [6. JavaScript SIP client using WebRTC](6_sip_webrtc.md)

## [Conclusion](Conclusion.md)

## Abbreviations

<table>
<thead>
<tr>
<th style="text-align:left; min-width: 128px">Abbreviation</th>
<th style="text-align:left;">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">802.11</td>
<td style="text-align:left;">IEEE 802.11 (Wi-Fi) standard.</td>
</tr>
<tr>
<td style="text-align:left;">802.3</td>
<td style="text-align:left;">IEEE 802.3 (Ethernet) standard.</td>
</tr>
<tr>
<td style="text-align:left;">ANSSI</td>
<td style="text-align:left;">Agence nationale de la sécurité des systèmes d&#39;information (French National Agency for the Security of Information Systems).</td>
</tr>
<tr>
<td style="text-align:left;">ECDSA</td>
<td style="text-align:left;">Elliptic Curve Digital Signature Algorithm, asymmetric digital signature algorithm using elliptic curve cryptography.</td>
</tr>
<tr>
<td style="text-align:left;">FTP</td>
<td style="text-align:left;">File Transfer Protocol.</td>
</tr>
<tr>
<td style="text-align:left;">HDMI</td>
<td style="text-align:left;">High-Definition Multimedia Interface.</td>
</tr>
<tr>
<td style="text-align:left;">HTTP</td>
<td style="text-align:left;">Hypertext Transfer Protocol.</td>
</tr>
<tr>
<td style="text-align:left;">HTTPS</td>
<td style="text-align:left;">HyperText Transfer Protocol Secure.</td>
</tr>
<tr>
<td style="text-align:left;">IEEE</td>
<td style="text-align:left;">Institute of Electrical and Electronics Engineers.</td>
</tr>
<tr>
<td style="text-align:left;">IP</td>
<td style="text-align:left;">Internet Protocol, network layer of the TCP/IP model.</td>
</tr>
<tr>
<td style="text-align:left;">IP PBX</td>
<td style="text-align:left;">Internet Protocol Private Vranch eXchange, this is PABX operating on the internet stack.</td>
</tr>
<tr>
<td style="text-align:left;">ISDN</td>
<td style="text-align:left;">Integrated Services Digital Network, digital telephone network with speeds of up to 2 Mbit/s (<a href="https://en.wikipedia.org/wiki/Integrated_Services_Digital_Network" hreflang="en" target="_blank">Wikipedia</a>).</td>
</tr>
<tr>
<td style="text-align:left;">LTS</td>
<td style="text-align:left;">Long Term Support, long term supported software/system version.</td>
</tr>
<tr>
<td style="text-align:left;">MAC</td>
<td style="text-align:left;">Media Access Control, data link layer protocol of the OSI model.</td>
</tr>
<tr>
<td style="text-align:left;">NAT</td>
<td style="text-align:left;">Network Address Translation.</td>
</tr>
<tr>
<td style="text-align:left;">P-521</td>
<td style="text-align:left;">Encryption algorithm using elliptic curves developed by the National Institute of Standards and Technology.</td>
</tr>
<tr>
<td style="text-align:left;">PBX</td>
<td style="text-align:left;">Private Branch eXchange, used to link telephone endpoints.</td>
</tr>
<tr>
<td style="text-align:left;">PHY</td>
<td style="text-align:left;">Physical layer of the OSI model.</td>
</tr>
<tr>
<td style="text-align:left;">PoE</td>
<td style="text-align:left;">Power over Ethernet, IEEE 802.3af standard, a PoE device allows a device to be powered over Ethernet while retaining the ability to transfer data.</td>
</tr>
<tr>
<td style="text-align:left;">PSTN</td>
<td style="text-align:left;">Public Switched Telephone Network, analog telephone network.</td>
</tr>
<tr>
<td style="text-align:left;">RFC</td>
<td style="text-align:left;">Request for comments, official document specifying Internet technologies.</td>
</tr>
<tr>
<tr>
<td style="text-align:left;">RPi</td>
<td style="text-align:left;">Raspberry Pi.</td>
</tr>
<tr>
<td style="text-align:left;">RSA</td>
<td style="text-align:left;">"RSA (Rivest–Shamir–Adleman) is a public-key cryptosystem that is widely used for secure data transmission. It is also one of the oldest. The acronym "RSA" comes from the surnames of Ron Rivest, Adi Shamir and Leonard Adleman" - <a href="https://en.wikipedia.org/wiki/RSA_(cryptosystem)" hreflang="en" target="_blank">Wikipedia</a>.</td>
</tr>
<tr>
<td style="text-align:left;">RTP</td>
<td style="text-align:left;">application protocol allowing, among other things, the transfer of audio or video streams.</td>
</tr>
<tr>
<td style="text-align:left;">SIP</td>
<td style="text-align:left;">Session Initiation Protocol, protocol establishing VoIP communication between two endpoints.</td>
</tr>
<tr>
<td style="text-align:left;">SIPS</td>
<td style="text-align:left;">Session Initiation Protocol over SSL/TLS.</td>
</tr>
<tr>
<td style="text-align:left;">SMTP</td>
<td style="text-align:left;">Simple Mail Transfer Protocol.</td>
</tr>
<tr>
<td style="text-align:left;">SRTP</td>
<td style="text-align:left;">Secure Real-time Transport Protocol.</td>
</tr>
<tr>
<td style="text-align:left;">SSH</td>
<td style="text-align:left;">Secure Shell, application communication protocol.</td>
</tr>
<tr>
<td style="text-align:left;">SSL</td>
<td style="text-align:left;">Secure Socket Layer, protocol for securing exchanges.</td>
</tr>
<tr>
<td style="text-align:left;">TCP</td>
<td style="text-align:left;">Transmission Control Protocol, transport layer protocol of the OSI model (connected mode).</td>
</tr>
<tr>
<td style="text-align:left;">TFTP</td>
<td style="text-align:left;">Trivial File Transfer Protocol, application protocol allowing the transfer of files by UDP.</td>
</tr>
<tr>
<td style="text-align:left;">TLS</td>
<td style="text-align:left;">Transport Layer Security, successor of SSL.</td>
</tr>
<tr>
<td style="text-align:left;">TZ</td>
<td style="text-align:left;">Teaching unit specific to the University of Technology of Compiègne, it consists of an experimental project carried out by a student, supervised by a teacher.</td>
</tr>
<tr>
<td style="text-align:left;">UDP</td>
<td style="text-align:left;">User Datagram Protocol, transport layer protocol of the OSI model (unconnected mode).</td>
</tr>
<tr>
<td style="text-align:left;">VM</td>
<td style="text-align:left;">Virtual Machine.</td>
</tr>
<tr>
<td style="text-align:left;">VoIP</td>
<td style="text-align:left;">Voice over Internet Protocol.</td>
</tr>
<tr>
<td style="text-align:left;">WebRTC</td>
<td style="text-align:left;">"WebRTC (Web Real-Time Communication) is a free and open-source project providing web browsers and mobile applications with real-time communication (RTC) via application programming interfaces (APIs)" - <a href="https://en.wikipedia.org/wiki/WebRTC" hreflang="en" target="_blank">Wikipedia</a>.</td>
</tr>
<tr>
<td style="text-align:left;">WS</td>
<td style="text-align:left;">"WebSocket is a computer communications protocol, providing full-duplex communication channels over a single TCP connection" - <a href="https://en.wikipedia.org/wiki/WebSocket" hreflang="en" target="_blank">Wikipedia</a>.</td>
</tr>
<tr>
<td style="text-align:left;">WSS</td>
<td style="text-align:left;">Web Socket Secure.</td>
</tr>
</tbody>
</table>

## [References](References.md)