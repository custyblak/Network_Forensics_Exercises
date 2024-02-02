<h2>Question 1 </h2>
<b> In order to effectively trace the attacker's activities within our network, can you determine the IP address of the machine where the attacker initially gained access?</b>

<br>Goto conversation, under statistics to see the flow of packets between endpoints within the capture. You will notice large flow of packets between 2 endpoints. Apply that conversation as a filter and scan through the capture, you will notice some frames with a particular source IP address shows the PsEXEC.EXEC within the info section. 

<p align="center">
<img src="https://imgur.com/1I6LBv9.png" height="100%" width="80%" alt="C2 Server"/> 
<br />

<h2>Question 2 </h2>
<b>To fully comprehend the extent of the breach, can you determine the machine's hostname to which the attacker first pivoted?</b>
<br>Using the filter applied to question 1, my approach was to follow the TCP stream and boom! the answer was at my face. My second approach was to check the protocol hierarcy for NetBios Name service but rather I saw NetBios session service and after a few googling, I understood that this service also uses the function of NetBios names for identification. So, I applied it (nbss) as a filter and opened one of the frames, the same answer was seen.<br />
<p align="center">
<img src="https://imgur.com/7khn9c1.png" height="100%" width="80%" alt="Entry_port"/> 
<br />

<h2>Question 3 </h2>
<b>After identifying the initial entry point, it's crucial to understand how far the attacker has moved laterally within our network. Knowing the username of the account the attacker used for authentication will give us insights into the extent of the breach. What is the username utilized by the attacker for authentication?</b>
<br>With the "nbss" filter applied, I noticed that the info on one of the frames had NTLMSSP_AUTH request, I clicked on the frame and there was my answer.

<p align="center">
<img src="https://imgur.com/QiB3Vs0.png" height="100%" width="80%" alt="Username"/>, 
<br />

<h2>Question 4 </h2>
<b>After figuring out how the attacker moved within our network, we need to know what they did on the target machine. What's the name of the service executable the attacker set up on the target?</b><br />
<br>With the same filter applied, you can either follow the TCP stream or following the info of the frames, you will noticed an executable file that was created by the attacker

<p align="center">
<img src="https://imgur.com/z0QCYdv.png" height="100%" width="80%" alt="Executable file"/> 
</br>

<h2>Question 5 </h2>
<b>We need to know how the attacker installed the service on the compromised machine to understand the attacker's lateral movement tactics. This can help identify other affected systems. Which network share was used by PsExec to install the service on the target machine?</b><br />
<br>Still on the applied filter, you will notice that before the executable was created on the victim's machine, the attacker made a connection request to an smb share which is clearly seen on the frames info.</br>
<p align="center">
<img src="https://imgur.com/t5C7LI7.png" height="100%" width="80%" alt="2nd smb-shares"/> 
<br/>

<h2>Question 6 </h2>
<b>We must identify the network share used to communicate between the two machines. Which network share did PsExec use for communication?</b><br />
<br>Using the same approach to question 5, before the attacker connected to the smb share in question 5, the attacker firstly connected to another and this can be seen in the info section of the frames.<br />

<p align="center">
<img src="https://imgur.com/Eqy4jze.png" height="100%" width="80%" alt="Initial_smb-shares"/> 
<br />

  
<h2>Question 7 </h2>
<b>Now that we have a clearer picture of the attacker's activities on the compromised machine, it's important to identify any further lateral movement. What is the machine's hostname to which the attacker attempted to pivot within our network?</b><br />
<br> My approach was to apply the "NetBIOS computer name" as a column and then observed when there is a change of name

<p align="center">
<img src="https://imgur.com/PDDvc8j.png" height="100%" width="80%" alt="@nd Hostname"/> 
<br />


<br />




