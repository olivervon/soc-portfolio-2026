<- [01-infrastructure](../01-infrastructure/readme.md)

# Attack Chain Preparation - Kali Attacker Node

## Objective

Deploy a dedicated attacker machine to simulate brute force and credential-based attacks against the domain environment.

## Kali Linux Configuration

- Hostname: KALI
- IP address: 192.168.100.50
- Network: SOC-LAB-NET
- DNS server: 192.168.100.10

The attacker machine is fully isolated within the lab network and will be used to generate authentication events for detection engineering.

## Evidence

### Kali VM Created

![kali vm created](screenshots/01-kali-vm-created.PNG)

### Kali Static IP Configuration

![kali static ip](screenshots/02-kali-static-ip.PNG)

## RDP Exposure on WIN10

Remote Desktop Protocol (RDP) was enabled on WIN10 to simulate an exposed authentication service.

This will be used to generate failed and successful authentication events from the attacker node.

### Evidence

![win10 rdp enabled](screenshots/03-win10-rdp-enabled.PNG)

## SMB Brute Force Simulation - rpcclient

A brute force attack was simulated from KALI against WIN10 over SMB (port 445) using rpcclient in a loop.

Command used:

for pass in $(cat passwords.txt); do rpcclient -U johjoh%$pass 192.168.100.30 -c "exit"; done

This generated multiple failed logon events in Windows Security logs.

- Event ID: 4625
- Logon Type: 3 (Network)
- Source IP: 192.168.100.50

This behavior represents a typical password spraying attack pattern.

### Evidence

![smb bruteforce 4625](screenshots/04-smb-bruteforce-4625.PNG)

## Credential Compromise - Successful SMB Authentication

After multiple failed authentication attempts (Event ID 4625), a successful login event was generated from the attacker node.

The attacker successfully authenticated using a password contained in the password list.

- Event ID: 4624
- Logon Type: 3 (Network)
- Account Name: johjoh
- Source IP: 192.168.100.50

This represents a successful credential compromise following a brute force / password spraying attack.

The event sequence demonstrates the transition from repeated authentication failures to a valid account compromise.

### Evidence

![credential success](screenshots/05-credential-success.PNG)

### Detection Considerations

The sequence of multiple 4625 events followed by a 4624 from the same source IP within a short time window is a strong indicator of successful brute force activity.

This pattern should trigger alerting logic in SIEM solutions such as Microsoft Sentinel.

## Lateral Movement - SMB Authentication to SRV01

Using the compromised domain credentials, authentication was performed against SRV01 over SMB.

Command used:

rpcclient -U SOCLAB/johjoh 192.168.100.20

This generated a successful authentication event on SRV01.

- Event ID: 4624  
- Logon Type: 3 (Network)  
- Source IP: 192.168.100.50  

This demonstrates lateral movement using valid domain credentials.

### Evidence

![lateral movement srv01](screenshots/06-lateral-movement-srv01.PNG)

### Detection Considerations

A successful 4624 event on a secondary host from the same source IP following a brute force sequence is a strong indicator of lateral movement using valid accounts.

Correlation between hosts is critical in SIEM detection logic.

## Privilege Escalation - Local Administrator Access on SRV01

To simulate privilege escalation, the compromised domain account was added to the local Administrators group on SRV01.

This represents a common post-compromise misconfiguration scenario.

Command used:

Add-LocalGroupMember -Group "Administrators" -Member "SOCLAB\johjoh"

Administrative privileges are required to access LSASS memory for credential dumping.

### Evidence

![privilege escalation](screenshots/07-privilege-escalation.PNG)

## Credential Dumping - LSASS Memory Dump

With local administrator privileges, LSASS memory was dumped using a native Windows technique.

Command used:

rundll32.exe C:\Windows\System32\comsvcs.dll, MiniDump <PID> C:\Windows\Temp\lsass2.dmp full

This simulates credential dumping behavior using built-in Windows binaries.

Security Event Observed:

- Event ID: 4688  
- Process Name: rundll32.exe  
- Parent Process: powershell.exe  
- Technique: LSASS Memory Dump  

MITRE ATT&CK Mapping:  
T1003.001 - OS Credential Dumping: LSASS Memory

### Evidence

![lsass dump 4688](screenshots/08-lsass-dump-4688.PNG)

### Detection Considerations

Execution of rundll32.exe with comsvcs.dll and MiniDump parameters targeting LSASS is a high-confidence indicator of credential dumping activity.

This behavior should trigger high-severity alerts in endpoint detection and SIEM solutions.

## Attack Chain Summary

1. Initial Access - SMB Brute Force (4625)
2. Credential Access - Successful Authentication (4624)
3. Lateral Movement - Authentication to SRV01 (4624)
4. Privilege Escalation - Local Administrator Access
5. Credential Dumping - LSASS Memory Dump (4688)

This lab demonstrates a complete on-prem attack chain from initial access to credential extraction.

## Next Phase

Continue to cloud monitoring integration:

-> [03-Cloud Integration](../03-cloud-integration/readme.md)
