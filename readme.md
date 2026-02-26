# SOC Analyst Portfolio - Hybrid Active Directory Detection Lab

This repository documents a hands-on SOC analyst lab focused on attack simulation, detection engineering, and incident investigation within a hybrid Active Directory environment.

The lab was designed to simulate realistic attacker activity and demonstrate how security events can be detected and investigated using Windows Security logs and Microsoft Sentinel.

The project follows a structured SOC workflow from initial compromise to incident investigation.

## Repository Navigation

- [Lab Environment](#lab-environment)
- [Environment Architecture](#environment-architecture)
- Repository Structure
  - [01 - Infrastructure](01-infrastructure)
  - [02 - Attack Chain](02-attack-chain)
  - [03 - Cloud Integration](03-cloud-integration)
  - [04 - Incident Investigation](04-incident-investigation)
- [Skills Demonstrated](#skills-demonstrated)
- [Project Objective](#project-objective)

## Lab Environment

The environment consists of a multi-system virtualized network simulating both attacker and enterprise infrastructure.

### On-Prem Infrastructure

- DC01 – Active Directory Domain Controller
- SRV01 – Member Server and investigation target
- WIN10 – Domain joined workstation
- Kali Linux – Attacker machine

Domain:

- soclab.local

### Cloud Integration

- Azure Arc enabled server
- Log Analytics Workspace
- Azure Monitor Agent
- Microsoft Sentinel SIEM

This architecture enables hybrid visibility between on-prem systems and cloud-based security monitoring.

## Repository Structure

### 01 - Lab Setup

Deployment of the Active Directory lab environment.

Focus areas:

- Domain configuration
- System preparation
- Logging baseline
- SOC lab foundation

### 02 - Attack Chain Simulation

Simulation of a realistic enterprise attack scenario.

Attack stages:

1. Initial Access – SMB brute force attempts from Kali Linux
2. Credential Compromise – Successful authentication
3. Lateral Movement – SMB authentication to SRV01
4. Privilege Escalation – Local administrator access
5. Credential Dumping – LSASS memory extraction

Primary security events analyzed:

- Event ID 4625 – Failed Logon
- Event ID 4624 – Successful Logon
- Event ID 4688 – Process Creation

This phase demonstrates attacker activity from a defensive monitoring perspective.

### 03 - Cloud Integration

Extension of on-premise logging into Azure.

Implemented components:

- Azure Arc onboarding
- Data Collection Rules
- Windows Security log ingestion
- Microsoft Sentinel integration
- Hybrid monitoring validation

Security telemetry from SRV01 is forwarded into the SIEM platform.

### 04 - Incident Investigation

SOC-style investigation of suspicious persistence activity.

Investigation workflow:

- Scheduled task persistence detection
- Process execution analysis
- Authentication correlation
- Attack timeline reconstruction
- MITRE ATT&CK mapping
- Analyst conclusion

Investigated events include:

- Event ID 4698 – Scheduled Task Creation
- Event ID 4688 – Process Execution
- Event ID 4624 – Authentication Activity

This phase reflects practical Tier-1 SOC investigation methodology.

## Skills Demonstrated

- Windows Security Log Analysis
- Active Directory Security Monitoring
- Threat Detection and Investigation
- Attack Chain Analysis
- Incident Response Investigation
- MITRE ATT&CK Mapping
- Microsoft Sentinel Fundamentals
- Hybrid SOC Monitoring
- Log Correlation and Timeline Analysis

## Project Objective

The objective of this portfolio is to demonstrate practical SOC analyst capabilities through realistic attack simulations and evidence-based investigations.

All offensive actions are performed solely to analyze detection visibility and defensive response from a SOC perspective.
