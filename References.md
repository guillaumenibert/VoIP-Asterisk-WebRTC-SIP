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


## [Abbreviations](Abbreviations.md)

## References

<p><a name="wikipedia_pabx"></a><strong>(1)</strong>: Wikipedia, <em>Business telephone system</em>, 21<sup>st</sup> january 2022, available at: <a href="https://en.wikipedia.org/wiki/Business_telephone_system" hreflang="en" target="_blank">https://en.wikipedia.org/wiki/Business_telephone_system</a>.</p>
<p><a name="q931"></a><strong>(2)</strong>: 
ITU Telecommunication Standardization Sector, <em>ISDN user-network interface layer 3 specification for basic call control</em>, ITU, May 1998, available at: <a href="https://www.itu.int/rec/T-REC-Q.931-199805-I/en" hreflang="en" target="_blank">https://www.itu.int/rec/T-REC-Q.931-199805-I/en</a>.</p>
<p><a name="x253"></a><strong>(3)</strong>: ITU Telecommunication Standardization Sector, <em>Interface between Data Terminal Equipment (DTE) and Data Circuit-terminating Equipment (DCE) for terminals operating in the packet mode and connected to public data networks by dedicated circuit</em>, ITU, October 1996, available at: <a href="https://www.itu.int/rec/T-REC-X.25-199610-I/en" hreflang="en" target="_blank">https://www.itu.int/rec/T-REC-X.25-199610-I/en</a>.</p>
<p><a name="sip_deprecie"></a><strong>(4)</strong>: Matt Fredrickson, <em>PSA: chan_sip status changed to “deprecated” &amp; Asterisk 17.0.0-rc2 Release</em>, Asterisk.org, 25<sup>th</sup> september 2019, available at: <a href="https://www.asterisk.org/deprecating-chan_sip-asterisk-17-0-0-rc2-release/" hreflang="en" target="_blank">https://www.asterisk.org/deprecating-chan_sip-asterisk-17-0-0-rc2-release/</a>.</p>
<p><a name="problematique_traversee_nat"></a><strong>(5)</strong>: Y. Yeryomin, F. Evers and J. Seitz, <em>II. The NAT and firewall problem</em> <strong>In</strong>: <em>Solving the firewall and NAT traversal issues for SIP-based VoIP</em>, International Conference on Telecommunications, p.1-2, July 2008, DOI: 10.1109/ICTEL.2008.4652645, available at: <a href="https://www.researchgate.net/publication/224341038_Solving_the_firewall_and_NAT_traversal_issues_for_SIP-based_VoIP" hreflang="en" target="_blank">https://www.researchgate.net/publication/224341038_Solving_the_firewall_and_NAT_traversal_issues_for_SIP-based_VoIP</a>.</p>
<p><a name="alaw_ulaw_alcatel"></a><strong>(6)</strong>: Alcatel-Lucent, <em>Audio characteristics</em> <strong>In</strong>: <em>Alcatel-Lucent IP Touch 4008/4018 Extended Edition Phones</em>, p.2, 2013, available at: <a href="https://assets.bmdstatic.com/assets/Data/brochure/SKU01413265_2.pdf" hreflang="en" target="_blank">https://assets.bmdstatic.com/assets/Data/brochure/SKU01413265_2.pdf</a>.</p>
<p><a name="alaw_ulaw_geo"></a><strong>(7)</strong>: Wikipedia, <em>G.711</em>, 20<sup>th</sup> august 2021, available at: <a href="https://en.wikipedia.org/wiki/G.711" hreflang="en" target="_blank">https://en.wikipedia.org/wiki/G.711</a>.</p>
<p><a name="extensions_conf"></a><strong>(8)</strong>: Malcolm Davenport, <em>Answer, Playback, and Hangup Applications</em> <strong>In</strong>: <em>Asterisk Documentation</em>, Asterisk.org, 19<sup>th</sup> december 2013, available at: <a href="https://wiki.asterisk.org/wiki/display/AST/Answer%2C+Playback%2C+and+Hangup+Applications" hreflang="en" target="_blank">https://wiki.asterisk.org/wiki/display/AST/Answer%2C+Playback%2C+and+Hangup+Applications</a>.</p>
<p><a name="rpi_benchmark"></a><strong>(9)</strong>: bluesman, <em>64 bit Raspberry Pi OS is here!</em>, Audiophile Style, 4<sup>th</sup> june 2020, available at: <a href="https://audiophilestyle.com/forums/topic/59499-64-bit-raspberry-pi-os-is-here/" hreflang="en" target="_blank">https://audiophilestyle.com/forums/topic/59499-64-bit-raspberry-pi-os-is-here/</a>.</p>
<p><a name="alcatel_conf"></a><strong>(10)</strong>: Alcatel-Lucent, <em>3.3. Initializing an IP Touch 40x8 EE phone</em> <strong>In</strong>: <em>IP Touch 4008/4018 Extended Edition - SIP Phone Installation Guide - 8AL90824AAAA ed02</em>, p.7-8, August 2010, available at: <a href="https://www.cluster2.hostgator.co.in/files/writeable/uploads/hostgator136107/file/iptouchsipphoneinstallationguide-ed02.pdf" hreflang="en" target="_blank">https://www.cluster2.hostgator.co.in/files/writeable/uploads/hostgator136107/file/iptouchsipphoneinstallationguide-ed02.pdf</a>.</p>
<p><a name="ecdsa"></a><strong>(11)</strong>: SECTIGO Store, <em>ECDSA vs RSA: Everything You Need to Know</em>, 9<sup>th</sup> june 2020, available at: <a href="https://sectigostore.com/blog/ecdsa-vs-rsa-everything-you-need-to-know/" hreflang="en" target="_blank">https://sectigostore.com/blog/ecdsa-vs-rsa-everything-you-need-to-know/</a>.</p>
<p><a name="ecdsa_anssi"></a><strong>(12)</strong>: Agence nationale de la sécurité des systèmes d'information, <em>Logarithme discret dans les courbes elliptiques définies sur GF(p)</em> <strong>In</strong>: <em>Référentiel Général de Sécurité version 2.0 - Annexe B1</em>, p.19-20, 21<sup>st</sup> february 2014, available at: <a href="https://www.ssi.gouv.fr/uploads/2014/11/RGS_v-2-0_B1.pdf" hreflang="fr" target="_blank">https://www.ssi.gouv.fr/uploads/2014/11/RGS_v-2-0_B1.pdf</a>.</p>

<p style="text-align: center;">
    <b>All references were consulted on 1<sup>st</sup> February 2022.</b>
</p>