<h1> Tomcat Takeover Blue Team Lab (Cyberdefender) </h1>

<h2>Scenerio</h2>
Our SOC team has detected suspicious activity on one of the web servers within the company's intranet. In order to gain a deeper understanding of the situation, the team has captured network traffic for analysis. This pcap file potentially contains a series of malicious activities that have resulted in the compromise of the Apache Tomcat web server. We need to investigate this incident further.<b/>

<br >NB: At the time of this documentation, this lab was active on [Cyberdefender](https://cyberdefenders.org/blueteam-ctf-challenges/135#nav-overview). Hence, no available walkthrough. 


<h2>Tools used include:</h2

- <b>Wireshark</b>
- <b>Networkminer</b>  


<h2>Question 1 </h2>
<b>Given the suspicious activity detected on the web server, the pcap analysis shows a series of requests across various ports, suggesting a potential scanning behavior. Can you identify the source IP address responsible for initiating these requests on our server?</b>


<p align="center">
<img src="https://imgur.com/SJohW8K.png" height="100%" width="80%" alt="Phishing Sample 1"/> 
<br />

The sample mail pretends to be legitimate coming from Microsoft (sender's address). However, the content of the mail focuses on **crypto**, which is absurd because the services rendered by Microsoft doesn't include cryptocurrency. Also, the closure signature references a different domain (Binance.com) and a [WHOIS](https://whois.domaintools.com) lookup on Binance.com, shows organization as DNStination Inc.

<p align="center">
<img src="https://imgur.com/TzGiIMj.png" height="100%" width="80%" alt="WHOIS INFO"/> 
<br />
  
Lets explore other indicators (from the start of the mail):<br />
- Multiple spelling errors (Inappropriate spacing within words)
- Created a sense of urgency in "**...the first 2500 to apply...**", "**The airdrop will end on July 29, 2023, 18:00 UTC, with a limited supply...**" and "**...on a first come, first served basis...**"
- Reward lure up to $80,000 mentioned to entice the user
- The "Join Airdrop" button redirects to a different destination that is not related to either "Binance.com" or "Microsoft.com" when hovered over. This will be investigated later.

Having seen the obvious indicators, let's now analyze the header information.

First and foremost, check the raw HTML to see if the domains of the "From" and "Return-path" matches. For this sample mail, it does.

<p align="center">
<img src="https://imgur.com/77VfvV7.png" height="100%" width="80%" alt="FROM INFO"/> <img src="https://imgur.com/leopLXa.png" height="100%" width="80%" alt="RETURN-PATH INFO"/>
<br />

Now, tools like **PhishTool** or **MxToolbox** can further be used to effienctly analyze the header information.

[MxToolbox](https://mxtoolbox.com/EmailHeaders.aspx): Goto mxtoolbox.com, click on "Analyze Headers" column, copy and paste the raw HTML header information into the space provided and then click on "Analyze header". See image below. 
<p align="center">
<img src="https://imgur.com/5xq1yh6.png" height="100%" width="80%" alt="WHOIS INFO"/> 
<br />

The Output comprises of:

- Email Authentication- SPF, DKIM & DMARC Compliance: From the output below, it is seen that the mail doesn't have any DKIM & DMARC records which adds to the suspicion. Because both are crucial in enhancing the security email communication, prevent phishing attacks, and build trust with both email service providers and recipients.
  
  <p align="center">
  <img src="https://imgur.com/xCxU89q.png" height="100%" width="80%"> 
  <br />
  
  <p align="center">
  <img src="https://imgur.com/OdH2b8e.png" height="100%" width="80%"> 
  <br />

- Email Path: This shows all intermediary SMTP servers inbetween the sender and receiver through which the mail was routed.
  <p align="center">
  <img src="https://imgur.com/uFwxD3q.png" height="100%" width="80%"> 
  <br />
- Other header information such as X-headers,
  <p align="center">
  <img src="https://imgur.com/aS3Xm2L.png" height="100%" width="80%"> 
  <br />
Similar information is also seen using **PhishTool**. However, the header information are splitted into sections- 
- Headers: This contains the basis information of the mail such as sender's & receiver's addresses, Reply-to, Timestamp, Reply-path
  <p align="center">
  <img src="https://imgur.com/cCz0tO1.png" height="100%" width="80%"> 
  <br />
- Received Lines: This lists the different hops through which the mail got to the receiver.
  <p align="center">
  <img src="https://imgur.com/8rJIlv3.png" height="100%" width="80%"> 
  <br />
- X-Headers: 
  <p align="center">
  <img src="https://imgur.com/wGj89eU.png" height="100%" width="80%"> 
  <br />
- Security: This shows the authentication results of SPF, DKIM & DMARC.
  <p align="center">
  <img src="https://imgur.com/MlavY8O.png" height="100%" width="80%"> 
  <br />
- Attachment & Message URLs: This shows embedded attachment and links within the body of the mail.
  <p align="center">
  <img src="https://imgur.com/O5Sf10S.png" height="100%" width="80%"> 
  <br />

  <p align="center">
  <img src="https://imgur.com/PH9ig4N.png" height="100%" width="80%"> 
  <br />
Let's analyze the embedded link.

As seen above, Phishtool shows any link on the body of the mail and that's one way to get URLs embedded in a mail. However, the other way is by checking the raw HTML of the email. But in this sample, the body of the mail is encoded in base64 as shown below and **CyberChef** will be used to decode and unveil the concealed URL.

<p align="center">
<img src="https://imgur.com/8CXliYI.png" height="100%" width="80%"> 
<br />
  
The recommended approach involves copying and pasting the raw HTML that encompasses both the email header and body into the input section of cyberchef, apply the "Extract URLs" recipe to retrieve the embedded URLs. But for this sample, given that the header isn't encoded, we are focusing solely on the body by excluding the mail header. Just copy and paste the encoded body of the mail, use a sequence of recipes namely "From Base64," "Regular expression (with URL built-in regex) selected," and "Extract URLs," the obscured URL is exposed. 
  
**NB:** The revealed URL has been defanged for safety measures, appearing as hxxps[://]click[.]pstmrk[.]it/3s/sweedbuy[.]com%2Fblog%2F/ahc/k_CuAQ/AQ/44a54f89-410d-4729-b21c-32c30d6eb945/1/qOoKiS9V1s?/23687658rodrigofp.

<p align="center">
<img src="https://imgur.com/89e0qvi.png" height="100%" width="80%"> 
<br />

Scan and analyze the suspicious URL using [**URLScan.io**](https://urlscan.io/). The output shows that the main IP is 52.16.132.0, located in Dublin, Ireland and belongs to AMAZON-02, US. The screenshot on the right is blank and further checks on the "HTTP" button displays **Status 404** (Webpage Unavailable) and that it is linked to a text document (reference the Type/MIME-Type under the HTTP tab). Checks on [AbuseIPDB](https://www.abuseipdb.com/check/) for the main ip address shows no reported abuse.

<p align="center">
<img src="https://imgur.com/7oEEhoF.png" height="100%" width="80%"> <img src="https://imgur.com/8gAf4Nw.png" height="100%" width="80%"> <img src="https://imgur.com/vwuyrbq.png" height="100%" width="80%">
<br />


Under the **Indicators** tab, you will notice a hash value (e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855), copy and paste it on [VirusTotal](https://www.virustotal.com/gui/file/e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855/behavior) search box and the result shows that the file is of 0 byte and zero detection. 

<p align="center">
<img src="https://imgur.com/ipQHnV9.png" height="100%" width="80%"> <img src="https://imgur.com/Pim5SY8.png" height="100%" width="80%">
<br />
  
However, under the **behaviour** tab, it is observed that some sandbox identified multiple malicious activities upon scanning the file to which the hash value belongs to. So, let's use [Any.run](https://app.any.run/tasks/b5b82219-8f4f-463b-98dd-39f7ba6cda4d) to analyze the behaviour of the link.

<p align="center">
<img src="https://imgur.com/ENqSMRb.png" height="100%" width="80%">
<br />

Sign into Any.run, click on new task, input the URL into the URL column and select the browser you want to use.

<p align="center">
<img src="https://imgur.com/VEkYBkV.png" height="100%" width="80%">
<br />

<p align="center">
<img src="https://imgur.com/UtKDOyc.png" height="100%" width="80%">
<br />

From the screenshot above, it is seen that the site is unavailable. However on the backgroud, multiple Network connections, HTTP/ DNS requests and chrome processes were made.

Furthermore, it is observed that all the HTTP requests were made to the "**edgedl.me.gvt1.com**" domain. How do we get the IP address of this domain? 

Search the DNS request tab using the domain name (edgedl.me.gvt1.com) or use the DNS lookup feature on [MxToolbox](https://mxtoolbox.com/SuperTool.aspx?action=a%3aedgedl.me.gvt1.com&run=toolpage) to get the IP address.

<p align="center">
<img src="https://imgur.com/p0hfHJh.png" height="100%" width="80%"> <img src="https://imgur.com/00GCrXW.png" height="100%" width="80%">
<br />

So, let's confirm the reputation of the host IP address on [VirusTotal](https://www.virustotal.com/gui/ip-address/34.104.35.123/detection) and [AbuseIPDB](https://www.abuseipdb.com/check/34.104.35.123)


From the results above, the domain's IP is waving a red flag, marked as malicious with multiple reports. Delving deeper into the network connection and DNS requests, reputation checks on most of the IPs there reveal they've got a record on AbuseIPDB. See snippet below.

<p align="center">
<img src="https://imgur.com/QoDx7DR.png" height="100%" width="80%"> <img src="https://imgur.com/ssnzdeT.png" height="100%" width="80%">
<br />

As earlier observed from the Any.run snippets above, there are multiple spawn up processes of chrome.exe and in one of the processes, an executable (.exe) file was dropped into the C:\Users\admin\AppData\Local\Temp\ directory. See snippet below.

<p align="center">
<img src="https://imgur.com/wJdSriX.png" height="55%" width="80%"> <img src="https://imgur.com/J4fSWGC.png" height="100%" width="80%">
<br />

Confirming the reputation of the file on [VirusTotal](https://www.virustotal.com/gui/file/dd2a954ce7131c5dfb2577430808f6aa5d5a364d8e37c06ad0fcf68442388cc4/detection) using its hash value, showed that it was detected by some YARA rules and also flagged as Trogan by 2 Security Vendors.

<p align="center">
<img src="https://imgur.com/qshy4I0.png" height="100%" width="80%"> <img src="https://imgur.com/09xprpg.png" height="100%" width="80%">
<br />

Also, [Hybrid-analyse](https://www.hybrid-analysis.com/sample/239bf419d29c335f028d26522da009ecd1c97abaa6b2316f8a32b904c4f2f32a/658d138b4057db6b780588c3) was used to analyze the link and the result showed some malicious/suspicious behaviours which were mapped to few MITRE ATT&CK Techniques and Tactics etc. See snippet below.

<p align="center">
<img src="https://imgur.com/V0UKCe0.png" height="100%" width="80%"> <img src="https://imgur.com/24BTsSa.png" height="100%" width="80%"> <img src="https://imgur.com/PefYbts.png" height="100%" width="80%"> <img src="https://imgur.com/nlojZPQ.png" height="100%" width="80%">
<br />

Haven seen the multiple red flags, we can conclude that it is a phishing mail with malicious intent.
