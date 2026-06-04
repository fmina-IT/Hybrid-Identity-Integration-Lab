# Hybrid Identity Integration Lab
> ⚠️ **Project Status: Under Active Development**
> This lab is currently being built and documented. Please check back for updates as I progress through hybrid sync configuration and identity governance testing.


## 📝 Project Summary
This project bridges the gap between on-premises Active Directory and modern cloud-based identity management. By synchronising the `LAB.local` environment with Microsoft Entra ID, I architected a hybrid identity ecosystem that supports seamless authentication and centralised management. This lab demonstrates advanced skills in identity lifecycle management, UPN transformation, hybrid device joining, and cloud-native synchronisation troubleshooting.

## 📌 Project Roadmap
* **Phase 1: Hybrid Readiness** — Domain verification, UPN suffix configuration, and preparation of the AD environment for synchronisation.
* **Phase 2: Entra Connect Integration** — Deployment and configuration of Microsoft Entra Connect to bridge local Active Directory with the M365 tenant.
* **Phase 3: Hybrid Identity & Device Management** — Implementing Hybrid Join, conditional access testing, and verifying identity synchronisation.

## 🚀 Technical Competencies
* **Hybrid Identity Governance:** Expertise in AD-to-Entra synchronisation, UPN transformations, and attribute-level object management.
* **Cloud Infrastructure & Security:** Hardening cloud-bound traffic via strictly controlled outbound-only NAT rules and implementing secure Hybrid Microsoft Entra Join.
* **Sync & Identity Lifecycle:** Proficiency in Entra Connect configuration, sync cycle monitoring, and diagnosing object attribute conflicts.
* **Operational Resilience:** Proactive monitoring and disaster recovery methodologies for identity services.

## 🛠️ Lab Stack & Tools
* **Virtualisation Platform:** Oracle VirtualBox
* **On-Premise Infrastructure:** Windows Server 2022 (DC01), 5 x Windows 11 Pro workstations
* **Cloud Infrastructure:** Microsoft 365 E3 Trial Tenant (Entra ID)
* **Synchronisation Tools:** Microsoft Entra Connect
* **Administration & Automation:** PowerShell, Entra Admin Center, AD Administrative Center

## 🌐 Infrastructure Topology
*[Insert Hybrid Identity Architecture Diagram Here]*

**Architectural Overview:** The environment utilises an Entra Connect sync agent to bridge the laboratory foundation with an Azure-based identity control plane. 
**Note on Security:** This hybrid configuration requires strictly controlled outbound firewall rules on the gateway to allow sync traffic while preventing unauthorised inbound access to the local DC.

---

## 🏗️ Phase 1: Hybrid Readiness
*(Document steps for Domain Verification and UPN Suffix setup here)*

## ☁️ Phase 2: Entra Connect Integration
*(Document Entra Connect installation and Sync Service Manager results here)*

## 🔐 Phase 3: Hybrid Identity & Device Management
*(Document Hybrid Join status for the 5-workstation fleet and first successful cloud sign-in with synced identities)*

## 🛠️ Troubleshooting & Operational Resilience
*(Document how you fixed "Sync errors," "UPN mismatch errors," or "AD object attribute issues.")*

---

## 🏁 Summary of Outcomes
Successfully migrated from a siloed on-premises environment to a functional Hybrid Identity ecosystem, enabling Single-Sign-On (SSO) and centralised cloud-resource management.

## 🚀 Future Roadmap
Future phases will focus on Conditional Access Policies, Multi-Factor Authentication (MFA) enforcement, and Intune device management to further harden the cloud security posture.
