<h1> Tomcat Takeover Blue Team Challenge </h1>

<h2>Scenerio</h2>
Our SOC team has detected suspicious activity on one of the web servers within the company's intranet. In order to gain a deeper understanding of the situation, the team has captured network traffic for analysis. This pcap file potentially contains a series of malicious activities that have resulted in the compromise of the Apache Tomcat web server. We need to investigate this incident further.

<br>NB: At the time of this documentation, this lab was active on [Cyberdefender](https://cyberdefenders.org/blueteam-ctf-challenges/135#nav-overview). Hence, I will showcase my approach to the solution and not explicitly the answers and it can be found [here](https://github.com/custyblak/PCAP-Network-Forensics/blob/main/Tomcat%20Takeover%20Blue%20Team%20LAB/Approach.md)

<h2>Tools used include:</h2>

- <b>Wireshark</b>
- <b>Networkminer</b>  


<h2>Challenge Questions </h2>
  <br />1. Given the suspicious activity detected on the web server, the pcap analysis shows a series of requests across various ports, suggesting a potential scanning behavior. Can you identify the source IP address responsible for initiating these requests on our server?<br />
  <br />2. Based on the identified IP address associated with the attacker, can you ascertain the city from which the attacker's activities originated?<br />
  <br />3. From the pcap analysis, multiple open ports were detected as a result of the attacker's activity scan. Which of these ports provides access to the web server admin panel?<br />
  <br />4. Following the discovery of open ports on our server, it appears that the attacker attempted to enumerate and uncover directories and files on our web server. Which tools can you identify from the analysis that assisted the attacker in this enumeration process?<br />
  <br />5. Subsequent to their efforts to enumerate directories on our web server, the attacker made numerous requests trying to identify administrative interfaces. Which specific directory associated with admin panel was the attacker able to uncover?<br />
  <br />6.Upon accessing the admin panel, the attacker made attempts to brute-force the login credentials. From the data, can you identify the correct username and password combination that the attacker successfully used for authorization?<br />
  <br />7. Once inside the admin panel, the attacker attempted to upload a file with the intent of establishing a reverse shell. Can you identify the name of this malicious file from the capture data?<br />
  <br />8. Upon successfully establishing a reverse shell on our server, the attacker aimed to ensure persistence on the compromised machine. From the analysis, can you determine the specific command they are scheduled to run to maintain their presence?


