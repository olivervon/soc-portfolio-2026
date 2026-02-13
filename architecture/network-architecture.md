# Network Architecture

## Objective

Design and deploy an isolated internal enterprise network in Hyper-V to simulate a realistic attack surface for SOC detection engineering.

---

## Network Design

A dedicated internal virtual switch was created in Hyper-V to simulate a flat enterprise LAN environment.

Network range:

192.168.100.0/24

Management Host (Hyper-V host):

192.168.100.1

This design allows:

- VM-to-VM communication
- Host-to-VM administrative access
- Complete isolation from external networks
- Controlled attack simulation

---

## Implementation Details

Virtual Switch Configuration:

- Type: Internal
- Name: SOC-LAB-NET

Host Network Adapter Configuration:

- IPv4 Address: 192.168.100.1
- Subnet Mask: 255.255.255.0
- Default Gateway: None
- DNS Server: None

---

## Evidence

![Internal Switch Created](screenshots/01-internal-switch-created.PNG) 
![Host IP Configured](screenshots/02-host-ip-configured.PNG)
![Host IP Verification](screenshots/03-host-ip-verification.PNG)

---

## Security Rationale

A flat internal network was intentionally selected to allow:

- Brute force authentication attempts
- Credential dumping scenarios
- Lateral movement between systems
- Log generation for detection validation

Network segmentation is intentionally absent to simulate common enterprise misconfigurations and enable realistic attack chain progression.

---

## Key Takeaways

- Network architecture directly impacts attack feasibility.
- Controlled isolation ensures safe offensive testing.
- A properly designed lab environment is foundational for detection engineering.
