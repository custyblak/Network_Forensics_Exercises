<h2>Question 1 </h2>
<b>By identifying the C2 IP, we can block traffic to and from this IP, helping to contain the breach and prevent further data exfiltration or command execution. Can you provide the IP of the C2 server that communicated with our server?</b>

<br>First of all, lets check to see the conversations that was captured in this PCAP. Under the statistics tab, navigate to the Conversions and you will notice that a particular communication involved large transmission of packets from the source IP address to a particular destination IP address on port 443 and given that data exfiltration is being investigated. So, this communication seems obvious.

<p align="center">
<img src="https://imgur.com/6yjWjrD.png" height="100%" width="80%" alt="C2 Server"/> 
<br />

<h2>Question 2 </h2>
<b>Initial entry points are critical to trace back the attack vector. What is the port number of the service the adversary exploited?</b>

<br>My first attempt was to download the [maxmind](https://www.maxmind.com/en/home) databases (GeoLite2 Country, GeoLite2 City and GeoLite2 ASN) onto Wireshark but this attempt was unsuccessful because the city wasn't populated. So, it resulted in using [IP2location.com](https://ip2location.com/demo/14.0.0.120) to get the name of the city.<br />
<p align="center">
<img src="https://imgur.com/KWEKvHX.png" height="100%" width="80%" alt="IP2location INFO"/> 
<br />

<h2>Question 3 </h2>
<b>Following up on the previous question, what is the name of the service found to be vulnerable?</b>
<br>

<p align="center">
<img src="https://imgur.com/z8Pe7HA.png" height="100%" width="80%" alt="Dst_port"/> 
<br />

<h2>Question 4 </h2>
<b>The attacker's infrastructure often involves multiple components. What is the IP of the second C2 server? </b><br />
<br> 

<p align="center">
<img src="https://imgur.com/TiajAMr.png" height="100%" width="80%" alt="Enumeration_tool"/> 
</br>

<h2>Question 5 </h2>
<b>Attackers usually leave traces on the disk. What is the name of the reverse shell executable dropped on the server?</b><br />
<br> 
<p align="center">
<img src="https://imgur.com/XdVB8w8.png" height="100%" width="80%" alt="Admin_directory"/> 
<br/>

<h2>Question 6 </h2>
<b>What is the vulnerable Java method and class that allows an attacker to run arbitrary code? (Format: Class.Method)</b><br />
<br> <br />

<p align="center">
<img src="https://imgur.com/OPcixPu.png" height="100%" width="80%" alt="Credentials"/> 
<br />

  
<h2>Question 7 </h2>
<b>To better understand the specific security flaw exploited, can you identify the CVE identifier associated with this vulnerability?</b><br />
<br>

<p align="center">
<img src="https://imgur.com/yANADqF.png" height="100%" width="80%" alt="Attacker's file_upload"/> 
<br />

<p align="center">
<img src="https://imgur.com/SgCoYCq.png" height="100%" width="80%" alt="Attacker's file_upload"/> 
<br />
<h2>Question 8 </h2>
<b>What is the vulnerable Java method and class that allows an attacker to run arbitrary code? (Format: Class.Method)</b><br />
<br> 
<p align="center">
<img src="https://imgur.com/QCZlkGx.png" height="100%" width="80%" alt="persistence_command"/> 
<br />



