<h2>Question 1 </h2>
<b> In order to effectively trace the attacker's activities within our network, can you determine the IP address of the machine where the attacker initially gained access?</b>

<br>

<p align="center">
<img src=".png" height="100%" width="80%" alt="C2 Server"/> 
<br />

<h2>Question 2 </h2>
<b>To fully comprehend the extent of the breach, can you determine the machine's hostname to which the attacker first pivoted?</b>
<br><br />
<p align="center">
<img src=".png" height="100%" width="80%" alt="Entry_port"/> 
<br />

<h2>Question 3 </h2>
<b>After identifying the initial entry point, it's crucial to understand how far the attacker has moved laterally within our network. Knowing the username of the account the attacker used for authentication will give us insights into the extent of the breach. What is the username utilized by the attacker for authentication?</b>
<br>

<p align="center">
<img src=".png" height="100%" width="80%" alt="Vuln_service1"/>, 
<br />
<p align="center">
<img src=".png" height="100%" width="80%" alt="Vuln_service2"/>
<br />

<h2>Question 4 </h2>
<b>After figuring out how the attacker moved within our network, we need to know what they did on the target machine. What's the name of the service executable the attacker set up on the target?</b><br />
<br>

<p align="center">
<img src=".png" height="100%" width="80%" alt="C2 server"/> 
</br>

<h2>Question 5 </h2>
<b>We need to know how the attacker installed the service on the compromised machine to understand the attacker's lateral movement tactics. This can help identify other affected systems. Which network share was used by PsExec to install the service on the target machine?</b><br />
<br></br>
<p align="center">
<img src=".png" height="100%" width="80%" alt="Executable_file"/> 
<br/>

<h2>Question 6 </h2>
<b>We must identify the network share used to communicate between the two machines. Which network share did PsExec use for communication?</b><br />
<br><br />

<p align="center">
<img src=".png" height="100%" width="80%" alt="Java_class"/> 
<br />

  
<h2>Question 7 </h2>
<b>Now that we have a clearer picture of the attacker's activities on the compromised machine, it's important to identify any further lateral movement. What is the machine's hostname to which the attacker attempted to pivot within our network?</b><br />
<br> 

<p align="center">
<img src=".png" height="100%" width="80%" alt="CVE_ID"/> 
<br />


<br />




