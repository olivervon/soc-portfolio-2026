<- [Back to Portfolio Overview](../)  
<- [02-attack-chain](../02-attack-chain/readme.md)

# Cloud Integration - Azure Arc and Microsoft Sentinel

## Objective

Extend the on-prem attack chain into Azure by forwarding Windows Security logs to Microsoft Sentinel.

This phase transitions the lab from standalone AD logging to hybrid cloud monitoring.

## Azure Architecture

On-Prem:
- DC01
- SRV01

Azure:
- Resource Group: rg-soc-lab
- Log Analytics Workspace: law-soc-lab
- Microsoft Sentinel enabled
- Azure Arc enabled server: SRV01

## Azure Arc Onboarding

SRV01 was onboarded to Azure Arc to allow hybrid management and monitoring.

Steps performed:
- Created Azure Arc connection script
- Installed Connected Machine Agent on SRV01
- Registered server in Azure
- Verified status: Connected

This allows Azure Monitor Agent to be installed for log forwarding.

## Current Status

- Sentinel activated
- SRV01 connected via Azure Arc
- Preparing Azure Monitor Agent installation

### Evidence

![srv01 arc connected](screenshots/01-srv01-arc-connected.PNG)

## Data Collection Rule - Windows Security Log

A custom Data Collection Rule (DCR) was created to forward Windows Security logs from SRV01 to Log Analytics.

Configuration:
- Data source: Windows Security log
- Collection type: Custom
- Target workspace: law-soc-lab
- Assigned resource: SRV01 (Azure Arc)

This enables cloud-based detection of authentication activity and credential dumping.

### Evidence

![dcr security log](screenshots/02-dcr-security-log.PNG)

![dcr resource association](screenshots/03-dcr-resource-association.PNG)

## Security Log Verification

Windows Security logs are successfully ingested via Azure Monitor Agent.

Security events are stored in the **Event** table.

Verified Event IDs:
- 4625 Failed logon
- 4624 Successful logon

### Evidence

![4625 visible](screenshots/04-4625-visible.PNG)

## Microsoft Sentinel Defender Portal Integration

Microsoft Sentinel is accessed through the Microsoft Defender portal as part of Microsoft's unified SOC platform.

### Evidence

![defender portal sentinel](screenshots/05-defender-portal-sentinel.PNG)

## Detection Capability Status

Windows Security events from the on-premise environment are successfully ingested into Microsoft Sentinel through Azure Arc and Azure Monitor Agent.

Authentication events including failed logons (Event ID 4625) are visible within the SIEM platform.

Due to ongoing Microsoft Sentinel migration to the Defender portal, analytics rule creation was not finalized within this lab phase.

However, the environment is fully prepared for detection engineering and incident creation.

The primary objective of validating hybrid log ingestion and visibility was successfully achieved.

## Next Phase

Proceed to SOC incident investigation:

-> [04-incident-investigation](../04-incident-investigation/readme.md)
