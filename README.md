# Critical Infrastructure Security — Build & Attack Simulation

A hands-on project simulating a critical infrastructure environment — PLC, SCADA/HMI, and
a segmented corporate network — covering both the engineering fundamentals and a full
adversary attack chain against it. Completed as part of a university critical infrastructure
security course.

## Overview

The simulated environment modelled a small industrial plant:
- A **PLC** running ladder logic for a process (tank fill/drain with level alarms and E-stop)
- A **SCADA/HMI** layer (FUXA) polling the PLC over Modbus TCP
- A **segmented network**: Enterprise → Site Business Systems → Operations & Control,
  separated by a firewall

I worked through this from two directions: building and understanding the control system,
then attacking it as an adversary would, end to end.

## Part 1 — Build & Understand

- Programmed PLC logic in multiple IEC 61131-3 languages (Ladder Diagram, Structured Text,
  Function Block Diagram, Sequential Function Chart)
- Built tank-fill control logic with high/low alarms and an emergency stop
- Connected a SCADA/HMI (FUXA) to the PLC and built an operator display
- Captured and analysed Modbus TCP, EtherNet/IP and S7comm traffic in Wireshark

## Part 2 — Attack Chain

| Stage | Technique |
|---|---|
| Reconnaissance | Passive protocol analysis, credential harvesting from clear-text captures |
| Scanning | Nmap/RustScan service and OS fingerprinting, ICS-specific NSE scripts |
| ICS exploitation | Unauthenticated Modbus reads/writes, EtherNet/IP fingerprinting, MiTM/DoS against SCADA |
| AD compromise | Kerberos username enumeration, AS-REP roasting, Kerberoasting, NetNTLMv2 capture (Responder) |
| Lateral movement | Evil-WinRM, Empire C2, privilege escalation via a stale local-admin account, pass-the-hash to Domain Admin |
| Pivoting | SSH tunneling / Proxychains through network segmentation into the OT zone |
| Final foothold | Anonymous FTP → uploaded reverse shell on the SCADA host |

## Key Finding

Almost every successful step in the attack chain depended on an **IT-hygiene failure**, not a
flaw in the control logic itself: reused credentials, an unnecessary remote-management service,
an undocumented firewall port-forward, a stale admin account, and unauthenticated legacy
protocols (Modbus, FTP). A single strong control at nearly any point — account lifecycle
management, network segmentation review, or disabling default services — would have broken
the chain.

## Standards & Frameworks Applied

- **IEC 62443** — zone/conduit segmentation, authentication (SR 1.1/1.2), integrity (SR 3.1),
  account management and least privilege
- **NIST SP 800-82** — ICS-specific risk management, segmentation, credential lifecycle
- **SOCI Act (Australia)** — critical infrastructure risk-management obligations

## Skills Demonstrated

`PLC programming` `SCADA/HMI` `Modbus / EtherNet-IP / S7comm` `Wireshark` `Nmap` `Active
Directory attacks` `Kerberoasting` `Responder` `Empire C2` `Mimikatz` `network pivoting` `IEC
62443` `NIST SP 800-82`

---
*Note: this repo is a sanitized summary. Lab-environment specifics (credentials, internal IP
ranges, screenshots) are omitted as they belong to the training platform used in coursework.*
