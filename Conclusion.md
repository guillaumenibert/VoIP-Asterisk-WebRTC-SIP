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

## Conclusion

<p style="text-align: justify; text-indent: 3em;">
This project allowed the development of a VoIP communication between a Raspberry Pi and an Alcatel IP Touch 4018 EE phone. We discovered new protocols such as SIP or RTP and the WebRTC API, which allows us to encapsulate SIP to establish a communication between several endpoints, using a web browser or not. It is easy to see the potential of IP for communications, and the need to use this type of technology in companies or public services.
</p>
<p style="text-align: justify; text-indent: 3em;">
This project could be improved by the implementation of encryption in particular with the SIPS protocol (SIP over SSL/TLS) at the level of the establishment of the communication between two end points (done in part when encapsulated in a WebSocket over TLS), but this could be integral, since the Alcatel supports SIP over TLS very well. Finally, the RTP protocol can also be encrypted, this is the SRTP (<i>Secure Real-time Transport Protocol</i>) when there is a call in progress. 
</p>
<p style="text-align: justify; text-indent: 3em;">
I would like to thank Mr Lounis for having proposed this experimental work, which was particularly enriching, as I was unaware of a large part of the functioning of IP telephony. This subject allowed me to manipulate both the back end and front end of the system and to measure the power of this architecture when it is integrated into the Internet network.
</p>

## [Abbreviations](Sigles.md)

## [References](References.md)