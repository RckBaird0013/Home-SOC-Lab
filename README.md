# Home-SOC-Lab

## First Home Lab Completed
Pictures committed. 

### This is just the beginning! 
I will continue to use my lab to better understand how to detect threats and how to create rules that are accurate. More to come from my newly set up SOC Analyst Lab.

Big thanks to Eric Capuano for the step by step on how to do so
https://blog.ecapuano.com/p/so-you-want-to-be-a-soc-analyst-intro

Note: Work is original to Eric Capuano's "SOC Analyst" environment and everything within this README file can be researched and sited at the link above. Eric is the original creator of this environment (Capuano, 2023)


### Preparation

The first step was to create my two Virutal Machines.

As instructed I used a virtual environment to set up my virtual machines. I signed up for a free VMware Workstation so the cost for me was nothing more than my time. All in all I believe the endeavor took roughly 5 hours to set up the whole lab as I ran into a couple issues during setup. I primarily used a YouTube video ( https://www.youtube.com/watch?v=oOzihldLz7U&t=1842s ) to setup the virutal machines and all the infrastructure. I found it to tedious having to pause and rewind to see things I missed after initial machine set up. Then I began using the blog provided my Eric to complete all the Command Line and Powershell terminal work. 

#### Attack Machine
I used a Linux Server Image as the attack machine as instructed due to its preinstalled packages. The Linux Server had to be configured with a static IP address so as to be easily accessible via Command Prompt which was completed within the Installer-config.yaml. Once this was completed SSH was easily usable via my own PC Command Prompt at ssh user@192.168.X.X.

![alt text](https://github.com/RckBaird0013/Home-SOC-Lab/blob/main/Virtual-Machines/Attack%20Machine.png)

#### Vulnerable Machine
I created a Windows Vulnerable Machine as the Victim. This served the purpose of using a very common machine and OS widely used today. I did have to disable many default features within the Windows VM. I disabled Microsoft Defender precisely as instructed by the in blog, especially since Windows Defender is capable of re-enabling if the proper steps aren't taken to disable it. This entailed turning many features such as Tamper Protection. After doing so I had to Disable Microsoft Defender within the Group Policy Editor and use of Adminstrator Command Prompt. Finally I had to permanently Disabled Defender via the Registry while also changing power configurations for regular use without timeouts. 

![alt text](https://github.com/RckBaird0013/Home-SOC-Lab/blob/main/Virtual-Machines/Defenseless%20Windows.png)

### Programs used during lab

#### Sysmons with SwiftOnSecurity's Configuration

Sysmon also has it's own built in Sigma Rules that help with detection and my learning purposes. 

#### LimaCharlie EDR

LimaCharlie EDR installation on Windows VM
This new platform is considered "Security Infrastructure as a Service". LimaCharlie had to have a sensor installed within the Windows VM and had to be configured to send Sysmon event logs along with its own EDR Telemetry. 

![alt text](https://github.com/RckBaird0013/Home-SOC-Lab/blob/main/Detection-and-Response/LC%20Overview%20with%20blur.png)

### Command and Control

#### C2 via SLIVER

Sliver is a C2 framework by BishopFox. I used this framework to create my first C2 session payload. Unfortunately for me this is where I ran into the majority of my problems. While I did follow the instructions for the creation of this payload I miskeyed the IP information in the implant which is why I was unable to gain C2 on my machine. After a bit of troubleshooting I started the process over for the creation of the payload which is when I discovered my error. Once I fixed the IP issue I was able to gain a control session onto my Windows Virtual Machine. With this control I was able to verify my implant and such things as privileges and discover my location within the virtual machine. 

##### Payload Creation as logged my LimaCharlie

![alt text](https://github.com/RckBaird0013/Home-SOC-Lab/blob/main/Attacking/Creation%20of%20Dramatic%20Cat.png)

##### Active Session
![alt text](https://github.com/RckBaird0013/Home-SOC-Lab/blob/main/Attacking/Active%20Session.png)

##### Netstat
![alt text](https://github.com/RckBaird0013/Home-SOC-Lab/blob/main/Attacking/Sysmon%20Green%20Highlights.png)



### Detetion and Response

LimaCharlie was a very easy to use and navigate. I was able to view Timelines, Processes, Network information and even the file systems being collected by the sensor. While the information I gather was instructed I know have a starting point to build off of so as to continue learning for the future. I did like the note mentioned by Eric Capuano in reference to SANS " you must first know normal before you can find evil". For me as I started I did not know what I was looking at, as I am new to LimaCharlie and SOC work in general, and now I know just a bit more. I realize that not all the information that is needed to accomplish this career change is learned bit by bit, byte by byte. I look forward to the challenge and hope that this serves as proof that I am willing to learn on my own and put in the time and work needed to succeed. 

#### DRAMATIC_CAT Payload created on Windows VM

![alt text](https://github.com/RckBaird0013/Home-SOC-Lab/blob/main/Detection-and-Response/Document%20Created.png)

#### Network_Connections

![alt text](https://github.com/RckBaird0013/Home-SOC-Lab/blob/main/Detection-and-Response/Detection.png)

#### Connection Established

![alt text](https://github.com/RckBaird0013/Home-SOC-Lab/blob/main/Detection-and-Response/Dramatic_Cat_Connection.png)

#### Sensitive Process Log

![alt text](https://github.com/RckBaird0013/Home-SOC-Lab/blob/main/Detection-and-Response/Sensitive%20Process%20within%20Timeline.png)

#### Hash Check with Virus Total 

Within LimaCharlie there is a quick function to check the Hash against known hashes. The fact that there is no match makes since because the implant was just created and secondly it was recently created and was not able to produce definitive answers. However, considering the widespread use of Virus Total not having a referential hash is a bit of a red flag. 

![alt text](https://github.com/RckBaird0013/Home-SOC-Lab/blob/main/Detection-and-Response/VirusTotal%20Scan%20Not%20found.png)

![alt text](https://github.com/RckBaird0013/Home-SOC-Lab/blob/main/Detection-and-Response/Dramatic_Cat_Hash_Check.png)

#### Payload search is possible via LimaCharlie.

![alt text](https://github.com/RckBaird0013/Home-SOC-Lab/blob/main/Detection-and-Response/Payload%20Search.png)

#### Credential Stealing and Prevention

I attempted to exfiltrate credentials. I was not successful. The attempt did begin logging these attempt which made it easier to create rules. 

Lastly I began efforts to use my attack machine to delete volume shadow copies. Again this did not work but it helps identify the issues within LimaCharlie dashboard.
This also proves that I was able to create a rule and verify the results so as to fine tune sensors to avoid False Positives and then finally boot them from their shell access.  

##### Ransomware

![alt text](https://github.com/RckBaird0013/Home-SOC-Lab/blob/main/Detection-and-Response/Ransomware%20attempt.png)

##### Rule created

![alt text](https://github.com/RckBaird0013/Home-SOC-Lab/blob/main/Detection-and-Response/Detection%20Rule%20Created.png)

##### Rule Trigger
![alt text](https://github.com/RckBaird0013/Home-SOC-Lab/blob/main/Detection-and-Response/Rule%20Triggered.png)

##### Logging captured
![alt text](https://github.com/RckBaird0013/Home-SOC-Lab/blob/main/Detection-and-Response/Detecting%20potential%20Ransomware.png)

##### Booted out
![alt text](https://github.com/RckBaird0013/Home-SOC-Lab/blob/main/Detection-and-Response/booted_out.png)

### References

Capuano, E. (2023, March 20) So you want to be a SOC Analyst? Part 4. https://blog.ecapuano.com/p/so-you-want-to-be-a-soc-analyst-part-1e0
