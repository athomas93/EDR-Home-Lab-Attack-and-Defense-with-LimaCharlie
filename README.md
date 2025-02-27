# EDR Home Lab: Attack and Defense

## Objective
This lab focuses on simulating a real-world cyber attack and testing endpoint detection and response (EDR). Following Eric Capuano's online guide, I will use virtual machines to replicate both threat and victim environments. The attack machine will employ 'Sliver' as a command-and-control (C2) framework to target a Windows endpoint, which will be protected by 'LimaCharlie' as the EDR solution.

Eric Capuano's Guide: <https://blog.ecapuano.com/p/so-you-want-to-be-a-soc-analyst-intro?utm_campaign=post&utm_medium=web>

### Tools Used
- VMWare NAT Network which was used to create the virtualized network environment
- Ubuntu Server and Windows 10 virtual machine's to simulate the attack and victim systems
- LimaCharlie EDR on the victim machine to enhance security monitoring and threat detection capabilities
- Sliver-Server installed on the Ubuntu Server as a Command & Control (C2) attack tool for exploitation

## Setting Up the Lab Environment
The attack machine will operate on Ubuntu Server, while the endpoint will run Windows 10. To ensure the lab functions properly, Microsoft Defender and certain other settings should be disabled. I will install Sliver on the Ubuntu machine as the primary attack tool and configure LimaCharlie on the Windows machine as the EDR solution. LimaCharlie will be connected to the Windows system via a sensor and will import Sysmon logs for monitoring.

Windows 10 Machine:
![Screenshot 2025-02-26 153145](https://github.com/user-attachments/assets/7487f87b-1ceb-40d3-aab3-d2e280fb45d7)
![Screenshot 2025-02-26 153047](https://github.com/user-attachments/assets/44bd0e1c-a73a-4e4a-9d43-32ad1cf3ed86)
![Screenshot 2025-02-26 222900](https://github.com/user-attachments/assets/54cc7fb8-e657-4934-a4a9-40d561a4eba7)

Ubuntu 22.04.1 Machine:
![Screenshot 2025-02-26 153401](https://github.com/user-attachments/assets/6c1bf824-b08f-414f-ad78-0a5e5b373a67)

## Executing the Attacks and Defense
The initial step involves generating a payload using Sliver and deploying the malware onto the Windows host machine. Once the malware is executed on the endpoint, a command-and-control session can be established.

![Screenshot 2025-02-26 153644](https://github.com/user-attachments/assets/41f0425c-6550-4e3a-94e2-cbb6d017f8a5)
![Screenshot 2025-02-26 163204](https://github.com/user-attachments/assets/b14a54e4-f075-4c1f-9574-6c32d35aef2e)
![Screenshot 2025-02-26 163323](https://github.com/user-attachments/assets/82dfb904-132f-4ddd-81b8-7d21c52794a8)

##
With an active session established between the two machines, the attack machine can now explore the system, verify privileges, gather host information, and assess the security measures in place.

![Screenshot 2025-02-26 163743](https://github.com/user-attachments/assets/f24444f6-ecff-4683-912b-293daddd9baa)
![Screenshot 2025-02-26 164507](https://github.com/user-attachments/assets/a839a91e-ab8e-4ac2-9e63-dee9e454db8e)

##
On the host machine, we can access the LimaCharlie SIEM to monitor telemetry data from the attacker. This allows us to identify the running payload and observe the IP address it is connected to.

![Screenshot 2025-02-26 165550](https://github.com/user-attachments/assets/00ecb088-b31e-4505-ae07-88500b85a26e)
![Screenshot 2025-02-26 165722](https://github.com/user-attachments/assets/8771cd25-ed1e-46bb-8375-b27e5ce1020f)

##
LimaCharlie also allows us to scan the payload’s hash using VirusTotal; however, the results will be clean since the payload was just created.

![Screenshot 2025-02-26 170541](https://github.com/user-attachments/assets/59486287-f89e-4359-86b4-ec9a6e03f6d0)

##
On the attack machine, we can simulate credential theft by dumping the LSASS memory. In LimaCharlie, we can monitor the sensors, analyze telemetry data, and create detection rules to identify this sensitive process.

![Screenshot 2025-02-26 175907](https://github.com/user-attachments/assets/ead3fef0-592d-4cd6-8f2c-fb75a45f8910)
![Screenshot 2025-02-26 183203](https://github.com/user-attachments/assets/158e14fb-038d-47f9-91b5-44cf39af41d4)
![Screenshot 2025-02-26 183501](https://github.com/user-attachments/assets/6482ce54-c632-4e57-b2e9-67172a10c9c2)

##
Now, rather than just detecting attacks, we can use LimaCharlie to create a rule that both identifies and blocks threats originating from the Sliver server. On the Ubuntu machine, we can simulate aspects of a ransomware attack by attempting to delete volume shadow copies. In LimaCharlie, we can analyze the telemetry data and implement a rule to completely prevent the attack. Once the rule is deployed in our SIEM, any further attempts from the Ubuntu machine to execute the same attack will be unsuccessful.

![Screenshot 2025-02-26 184947](https://github.com/user-attachments/assets/5515cac7-729e-48f8-8593-164e5cb03b83)
![Screenshot 2025-02-26 185114](https://github.com/user-attachments/assets/624a8ba7-30f2-4a37-b62b-b57c286aa55d)
![Screenshot 2025-02-26 185846](https://github.com/user-attachments/assets/f98b41bc-f92e-4ee8-b335-0664d26097f0)
![Screenshot 2025-02-26 190018](https://github.com/user-attachments/assets/5052adff-e260-48e4-acce-9e4d3287ec05)


