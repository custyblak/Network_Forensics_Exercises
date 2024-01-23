<h1> Tomcat Takeover Blue Team Lab (Cyberdefender) </h1>

<h2>Scenerio</h2>
Our SOC team has detected suspicious activity on one of the web servers within the company's intranet. In order to gain a deeper understanding of the situation, the team has captured network traffic for analysis. This pcap file potentially contains a series of malicious activities that have resulted in the compromise of the Apache Tomcat web server. We need to investigate this incident further.</b>

<b><br>NB: At the time of this documentation, this lab was active on [Cyberdefender](https://cyberdefenders.org/blueteam-ctf-challenges/135#nav-overview). Hence, no available walkthrough.</b> Sign into your Cyberdefender account, navigate to the practice lab section and download the Pcap file.</br>

<h2>Tools used include:</h2

- <b>Wireshark</b>
- <b>Networkminer</b>  


<h2>Question 1 </h2>
<b>Given the suspicious activity detected on the web server, the pcap analysis shows a series of requests across various ports, suggesting a potential scanning behavior. Can you identify the source IP address responsible for initiating these requests on our server?</b>

<br>So, by scanning through the pcap file, you can easily identify multiple syn packets from an IP address to a particular destination IP address but with varying destination ports. Also, you can use this wireshark filter <b>((http.request or tls.handshake.type eq 1 or tcp.flags eq 0x0002) and !(ssdp))</b> to identify the attacker's IP address

<p align="center">
<img src="https://imgur.com/Rr4qOOA" height="100%" width="80%" alt="Attacker's IP addr"/> 
<br />

<h2>Question 2 </h2>
<b>Based on the identified IP address associated with the attacker, can you ascertain the city from which the attacker's activities originated? </b>

<br>My first attempt was to download the [maxmind](https://www.maxmind.com/en/home) databases (GeoLite2 Country, GeoLite2 City and GeoLite2 ASN) onto Wireshark but this attempt was unsuccessful because the city wasn't populated. So, it resulted in using [IP2location.com](https://ip2location.com/demo/14.0.0.120) to get the name of the city.
<p align="center">
<img src="https://imgur.com/KWEKvHX" height="100%" width="80%" alt="IP2location INFO"/> 
<br />

<h2>Question 3 </h2>
<b>From the pcap analysis, multiple open ports were detected as a result of the attacker's activity scan. Which of these ports provides access to the web server admin panel?</b>
<br>My approach was filtering using HTTP GET request with the attacker's IP address containing "admin" related frames <b>((ip.src eq 14.0.0.120) && (http.request.method == "GET") and frame contains "admin")</b>. Then, note the destination port.
<p align="center">
<img src="https://imgur.com/z8Pe7HA" height="100%" width="80%" alt="Dst_port"/> 
<br />

<h2>Question 4 </h2>
<b>Following the discovery of open ports on our server, it appears that the attacker attempted to enumerate and uncover directories and files on our web server. Which tools can you identify from the analysis that assisted the attacker in this enumeration process? </b>
<br> From the answer to question 3, click on the frame and follow either the TCP or HTTP stream. Pay attention to the user agent type and google it to get more information
<p align="center">
<img src="https://imgur.com/TiajAMr" height="100%" width="80%" alt="Enumeration_tool"/> 
<br />

<h2>Question 5 </h2>
<b>Subsequent to their efforts to enumerate directories on our web server, the attacker made numerous requests trying to identify administrative interfaces. Which specific directory associated with admin panel was the attacker able to uncover?</b>
<br> Also from question 4, Strolling through the TCP or HTTP stream that was followed, you will see an <b>HTTP response code 401: Unauthorized</b> . Above it is the directory that was brute-forced which returned the said response code.
<p align="center">
<img src="https://imgur.com/XdVB8w8" height="100%" width="80%" alt="Admin_directory"/> 
<br />

<h2>Question 6 </h2>
<b>Upon accessing the admin panel, the attacker made attempts to brute-force the login credentials. From the data, can you identify the correct username and password combination that the attacker successfully used for authorization?</b>
<br> From the previous answer, it is known what the admin directory is.  .
<p align="center">
<img src="https://imgur.com/XdVB8w8" height="100%" width="80%" alt="Admin_directory"/> 
<br />

<h2>Question 7 </h2>
<b>Once inside the admin panel, the attacker attempted to upload a file with the intent of establishing a reverse shell. Can you identify the name of this malicious file from the capture data?</b>
<br> Also, from question 4, Strolling through the TCP or HTTP stream that was followed, you will see an <b>HTTP response code 401: Unauthorized</b> . Above it is the directory that was brute-forced which returned the said response code.
<p align="center">
<img src="https://imgur.com/XdVB8w8" height="100%" width="80%" alt="Admin_directory"/> 
<br />

<h2>Question 8 </h2>
<b>Upon successfully establishing a reverse shell on our server, the attacker aimed to ensure persistence on the compromised machine. From the analysis, can you determine the specific command they are scheduled to run to maintain their presence?</b>
<br> Also, from question 4, Strolling through the TCP or HTTP stream that was followed, you will see an <b>HTTP response code 401: Unauthorized</b> . Above it is the directory that was brute-forced which returned the said response code.
<p align="center">
<img src="https://imgur.com/XdVB8w8" height="100%" width="80%" alt="Admin_directory"/> 
<br />
