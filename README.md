# Hybrid-Identity-Integration-Lab

## 📝 Summary
This project extends the foundational on-premises infrastructure into a hybrid cloud environment. By synchronising the `LAB.local` identity database with Microsoft Entra ID (formerly Azure AD), I demonstrated enterprise-grade identity lifecycle management, UPN transformation, and hybrid device join capabilities, effectively bridging local domain security with cloud-based resource access.

## 📌 Project Roadmap

* **Phase 1: Hybrid Readiness** — Domain verification, UPN suffix configuration, and preparation of the AD environment for synchronisation.
* **Phase 2: Entra Connect Integration** — Deployment and configuration of Microsoft Entra Connect to bridge local Active Directory with the M365 tenant.
* **Phase 3: Hybrid Identity & Device Management** — Implementing Hybrid Join, conditional access testing, and verifying identity synchronisation.

## 🎯 Project Objectives

* **Hybrid Identity Architecture:** Architected a secure bridge between on-premises AD DS and Entra ID to enable unified identity management.
* **Synchronisation Management:** Configured and troubleshot the Entra Connect sync engine to ensure accurate, timely propagation of users, groups, and device objects.
* **Identity Lifecycle Management:** Standardised UPN suffix management to ensure seamless authentication across on-premises and cloud-based M365 services.
* **Hybrid Security Posture:** Validated Hybrid Microsoft Entra Join, allowing for device-based compliance policies and secure conditional access testing.
* **Cloud-Native Troubleshooting:** Developed methodologies for diagnosing sync errors, object attribute conflicts, and connectivity issues between local infrastructure and Entra ID.

## 🧰 Lab Stack & Tools

- **Virtualisation Platform:** Oracle VirtualBox
- **On-Premise Infrastructure:** Windows Server 2022 (DC01), Windows 11 (WS01)
- **Cloud Infrastructure:** Microsoft 365 E3 Trial Tenant (Entra ID)
- **Synchronisation Tools:** Microsoft Entra Connect
- **Administration & Automation:** PowerShell, Azure AD/Entra Admin Center, AD Administrative Center

## 🌐 Infrastructure Specs

| Component | Role/Function |
|---|---|
| Domain Name | `LAB.local` (Local) / `yourname.com` (Cloud UPN) |
| Entra Connect | Sync engine (deployed on DC01 or dedicated server) |
| Synchronisation | Password Hash Synchronisation (PHS) |
| Network | Dual-homed DC with NAT access for cloud synchronisation |

## 🌐 Infrastructure Topology

*[Insert Hybrid Identity Architecture Diagram Here]*

**Architectural Overview:** The environment utilises an Entra Connect sync agent to bridge the air-gapped laboratory foundation with an Azure-based identity control plane. Synchronisation is performed via a dedicated outbound NAT path, allowing local Active Directory to remain the authoritative source while enabling cloud-based authentication.

**Note on Security:** This hybrid configuration requires strictly controlled outbound firewall rules on the gateway to allow sync traffic while preventing unauthorised inbound access to the local DC.

---

## 🏗️ Phase 1: Hybrid Readiness
*(Document your steps for Domain Verification and UPN Suffix setup here)*

---

## ☁️ Phase 2: Entra Connect Integration
*(Document your Entra Connect installation and Sync Service Manager results here)*

---

## 🔐 Phase 3: Hybrid Identity & Device Management
*(Document Hybrid Join status for WS01 and your first successful cloud sign-in with a synced identity)*

---

## 🛠️ Troubleshooting & Operational Resilience
*(Document how you fixed "Sync errors," "UPN mismatch errors," or "AD object attribute issues.")*

## 🔑 Key Skills Demonstrated

* **Hybrid Identity Governance:** Expert-level understanding of AD-to-Entra synchronisation processes.
* **Cloud Infrastructure Hardening:** Implementing secure outbound-only traffic for sync services.
* **Identity Mapping:** Proficiency in UPN transformations and attribute-level object management.
* **Operational Resilience:** Proactive monitoring of sync cycles and disaster recovery for identity services.

## 🏁 Summary of Outcomes
Successfully migrated from a siloed on-premises environment to a functional Hybrid Identity ecosystem, enabling single-sign-on (SSO) and centralised cloud-resource management.

## 🚀 Next Steps/Future Roadmap
Future phases will focus on Conditional Access Policies, Multi-Factor Authentication (MFA) enforcement, and Intune device management to further harden the cloud security posture.
