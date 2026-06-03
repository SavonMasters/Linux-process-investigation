# Linux-process-investigation


Title: Linux process investigation 
Date: May 16th 2026
Analyst: Savon Masters


					                                 Summary 

Processes are ran countless times on a system, to catch ones that are impactful you have to see the difference between dangerous and safe items. I simulated an attack in the way of a process, I investigated it to find the root, and replied with action to the attack.	
		
                                                Investigation 

![image alt](https://github.com/SavonMasters/Linux-process-investigation/blob/cf0ae3c96385c371a37fcb4de5fb4814ddc770bc/Screenshot%20from%202026-05-16%2017-45-58.png)
I inserted “cat /dev/urandom > dev/null” into the command line to make an event with high CPU usage.


![image alt](https://github.com/SavonMasters/Linux-process-investigation/blob/be0259d852b0d974ed0e1df49ccfc6456ca8b8d6/Screenshot%20from%202026-05-16%2017-56-36.png)
![image alt](https://github.com/SavonMasters/Linux-process-investigation/blob/a6b3e0f539e8cbb2af19ac6c2e7ea8b55b155513/Screenshot%20from%202026-05-16%2019-24-39.png)
On the system I find all processes information by using the “ps aux –sort=-%cpu | head” One stood out at the beginning for a few different errors; the command being ran is “cat /dev/urandom”, cat is not a typical process that is ran and /dev/urandom prompts the cryptographically secure pseudorandom number generator (CSPRNG) to generate ongoing amount of numbers to a system which would be effective for a DOS attack. Other than the command the CPU is very high for an individual process ”95.9%”.  With the other data I could see the process ID  “91112”.




When I identify the process ID  I can build piece by piece the process's family tree. I used the “pstree 91112” to find the parent process, the “readlink -f /proc/91112/exe” to find the exe location on the system and the  “cat /proc/91112/cmdline”  to view the  process’s original command.




To shield the system and restore it I set up the “kill -19 91112 and kill -9 91112” to suspend and kill the process. It needs to be determined if the process was destroyed, I ran the same command from before and there was no copy of the process. 


													Conclusion 

To end, an attack with a process took place, it’s makeup was high CPU usage and making the host extend itself for connectivity. I pulled back the process’s user creation, ID, location, commandline, and parent process and enforced safety response of the kill and suspension on the process. 


						                                 IOCS

* The process “cat /dev/urandom > /dev/null”  being ran, 
* The use of cat in the command line of the process.
* Rest of the processes using normal CPU usage while one is taking “95.9%”

				                                        Recommendations 

* Develop an endpoint detection alert to go over all processes CPU usage goes beyond 80%.
* Keep monitoring processes commands it will show attacks placed on system.
* It only needs to be short amount of elevated accounts with executions powers to avoid malicious behavior.


					
                                                Things I learned 

* Have caution with processes with increased CPU usage to watch for different processes.
* A processes overview through connected commands.
* A better paced way to respond to an incident without regrowing another one.
* A process being at the top of the chain for an attacker to cause damage to a system. 
