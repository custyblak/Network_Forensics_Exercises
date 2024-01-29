
<h1> PsExec Hunt Lab Challenge </h1>

<h2>Scenerio</h2>
Our Intrusion Detection System (IDS) has raised an alert, indicating suspicious lateral movement activity involving the use of PsExec. To effectively respond to this incident, your role as a SOC Analyst is to analyze the captured network traffic stored in a PCAP file.

<br>NB: At the time of this documentation, this lab was active on [Cyberdefender](https://cyberdefenders.org/blueteam-ctf-challenges/153#tab=details). Hence, I will showcase my approach to the solutions and not explicitly the answers and it can be found [here](https://github.com/custyblak/Network_Forensics_Exercises/blob/main/Cyberdefender/PsExec%20Hunt%20Challenge/Approach.md)

<h2>Tool used:</h2>

- <b>Wireshark</b>


<h2>Challenge Questions </h2>
  <br />1. In order to effectively trace the attacker's activities within our network, can you determine the IP address of the machine where the attacker initially gained access?<br />
  <br />2. To fully comprehend the extent of the breach, can you determine the machine's hostname to which the attacker first pivoted?<br />
  <br />3. After identifying the initial entry point, it's crucial to understand how far the attacker has moved laterally within our network. Knowing the username of the account the attacker used for authentication will give us insights into the extent of the breach. What is the username utilized by the attacker for authentication?<br />
  <br />4. After figuring out how the attacker moved within our network, we need to know what they did on the target machine. What's the name of the service executable the attacker set up on the target?<br />
  <br />5. We need to know how the attacker installed the service on the compromised machine to understand the attacker's lateral movement tactics. This can help identify other affected systems. Which network share was used by PsExec to install the service on the target machine?<br />
  <br />6. We must identify the network share used to communicate between the two machines. Which network share did PsExec use for communication?<br />
  <br />7. Now that we have a clearer picture of the attacker's activities on the compromised machine, it's important to identify any further lateral movement. What is the machine's hostname to which the attacker attempted to pivot within our network?<br />
  



