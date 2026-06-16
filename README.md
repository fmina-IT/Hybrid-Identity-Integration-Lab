# Hybrid Identity Integration Lab

## 📝 Project Summary
This project bridges the gap between on premises Active Directory and modern cloud based identity management. By synchronising the `LAB.local` environment with Microsoft Entra ID, I architected a hybrid identity ecosystem that supports seamless authentication and centralised management. This lab demonstrates advanced skills in identity lifecycle management, UPN transformation, hybrid device joining, and cloud native synchronisation troubleshooting.

**Note on Domain Configuration:** This lab utilises the default FrankMinaITLab.onmicrosoft.com domain for cost efficiency. In a production environment, a verified, custom public domain would be implemented to ensure professional branding and simplified end user authentication.

## 📌 Project Roadmap
* **Phase 1: Hybrid Readiness** — Domain verification, UPN suffix configuration, and preparation of the AD environment for synchronisation.
* **Phase 2: Entra Connect Integration** — Deployment and configuration of Microsoft Entra Connect to bridge local Active Directory with the M365 tenant.
* **Phase 3: Hybrid Identity & SSO Validation** — Validate that synchronised identities are correctly provisioned and that Hybrid Microsoft Entra ID Join and Seamless SSO configurations provide a seamless authentication experience.

## 🚀 Technical Competencies
* **Hybrid Identity Governance:** Expertise in AD-to-Entra synchronisation, UPN transformations, and attribute level object management.
* **Cloud Infrastructure & Security:** Hardening cloud bound traffic via strictly controlled outbound only NAT rules and implementing secure Hybrid Microsoft Entra Join.
* **Sync & Identity Lifecycle:** Proficiency in Entra Connect configuration, sync cycle monitoring, and diagnosing object attribute conflicts.
* **Operational Resilience:** Proactive monitoring and disaster recovery methodologies for identity services.

## 🛠️ Lab Stack & Tools
* **Virtualisation Platform:** Oracle VirtualBox
* **On Premise Infrastructure:** Windows Server 2022 (DC01), 5 x Windows 11 Pro workstations
* **Cloud Infrastructure:** Microsoft 365 E3 Trial Tenant (Entra ID)
* **Synchronisation Tools:** Microsoft Entra Connect
* **Administration & Automation:** PowerShell, Entra Admin Center, AD Administrative Center

## 🌐 Infrastructure Topology
*[Insert Hybrid Identity Architecture Diagram Here]*

**Architectural Overview:** The environment utilises an Entra Connect sync agent to bridge the laboratory foundation with an Azure based identity control plane. 
**Note on Security:** This hybrid configuration requires strictly controlled outbound firewall rules on the gateway to allow sync traffic while preventing unauthorised inbound access to the local DC.

---

## 🏗️ Phase 1: Hybrid Readiness
---

**Objective:** Prepared the on premises environment for cloud synchronisation by aligning UPN suffixes and performing data hygiene audits.

### 1.1 Domain Verification
Before syncing local identities, I verified the domain in the Microsoft Entra admin center to ensure it is ready to accept synchronised accounts.

*   **Status:** Healthy
<img src="https://i.imgur.com/RsnsVGV.png" alt="Image Description">

### 1.2 UPN Suffix Configuration
To ensure a consistent user experience between on premises and the cloud, I added `FrankMinaITLab.onmicrosoft.com` as an alternative UPN suffix in Active Directory Domains and Trusts.
| Before | After |
| :--- | :--- |
| ![Image Description](https://i.imgur.com/GOHyI5S.png) | ![Image Description](https://i.imgur.com/zgl4DOw.png) |

### 1.3 User Alignment
I updated the User Principal Name (UPN) for test accounts to match the cloud verified domain. This allows the Entra Connect sync agent to correctly map local identities to the cloud tenant.

| Before | After |
| :--- | :--- |
| ![Image Description](https://i.imgur.com/qg7v0Gl.png) | ![Image Description](https://i.imgur.com/Rawxiqg.png) |

### 1.4 Data Hygiene Audit
I performed a manual audit of Active Directory attributes to ensure UPN uniqueness and compliance.

*Note: The Microsoft IdFix tool, traditionally used for this process, is being retired on June 30, 2026, with no further development or security updates. To ensure long term viability for this lab, I utilized manual PowerShell auditing rather than relying on legacy software.*

**Command Used:**
`Get-ADUser -Filter 'UserPrincipalName -notlike "*"'`

**Result:**
The audit identified only three default system accounts (`Administrator`, `Guest`, `krbtgt`) lacking a UPN. These are built in system objects and are excluded from synchronisation by default. All human provisioned accounts were verified to have correctly configured UPNs, ensuring they are ready for cloud synchronisation.
<img src="https://i.imgur.com/jQmhQnK.png" alt="Image Description">

---

## ☁️ Phase 2: Hybrid Identity Synchronisation
---

**Objective:** Established a hybrid identity bridge by deploying Microsoft Entra Connect to synchronise on premises Active Directory objects with the Microsoft Entra cloud tenant.

### 2.1 Synchronisation Deployment
I chose to deploy the traditional **Microsoft Entra Connect Sync** (Connect Sync) to establish a secure synchronisation bridge between the on premises environment and the cloud. 

*Rationale:* While Microsoft recommends Cloud Sync for modern, lightweight deployments, I selected the classic Connect Sync for this lab because it remains the industry standard for complex hybrid environments. Mastering this agent provides deeper insight into synchronisation rules, attribute mapping, and the core mechanics of identity propagation.

**Architectural Note:** 
While Microsoft best practices recommend isolating the Entra Connect Sync service on a dedicated domain joined member server to ensure separation of functions and resource security, this lab environment consolidates the sync agent onto `DC01` to optimise for resource efficiency. This design choice prioritises minimising the hardware footprint while maintaining full functional parity with an enterprise level hybrid identity deployment.

Configuration Note: > "I chose the 'Customise' installation path instead of Express settings. This was necessary to correctly handle the non routable .local Active Directory forest and ensure that UPN mapping is explicitly configured to align with my routable cloud domain, preventing authentication conflicts."
![Image Description](https://i.imgur.com/GxqaRKQ.png)
<p align="center">
  <img src="https://i.imgur.com/vSTPV8y.png" alt="Image Description" width="700">
</p>

### 2.2 Identity Preparation (Bulk Update)
To ensure all user identities were compliant with the verified routable cloud domain, I performed a bulk UPN suffix update using PowerShell. This ensured that all local identities were mapped correctly for synchronisation, replacing the non routable `@LAB.local` suffix with the routable `@FrankMinaITLab.onmicrosoft.com` suffix.

**Automation Script:**
Get-ADUser -Filter 'UserPrincipalName -like "*@LAB.local"' | ForEach-Object {
    $newUpn = $_.UserPrincipalName -replace "@LAB.local", "@FrankMinaITLab.onmicrosoft.com"
    Set-ADUser -Identity $_.DistinguishedName -UserPrincipalName $newUpn
}

<img src="https://i.imgur.com/qcNrybf.png" alt="Image Description">

### 2.3 User Sign-In Configuration
I configured the environment to use **Password Hash Synchronisation** combined with **Single Sign-On (SSO)**. 

*Rationale:* Password Hash Sync is the most robust and widely used authentication method for hybrid environments, offering excellent reliability with minimal infrastructure overhead. Enabling SSO allows for a seamless authentication experience, mimicking a mature enterprise environment where users are automatically signed into cloud services via their corporate credentials.
<p align="center">
  <img src="https://i.imgur.com/FDoFfmU.png" alt="Image Description" width="700">
</p>

### 2.4 Tenant Authentication
I authenticated the synchronisation agent against my Microsoft Entra ID tenant using a Global Administrator account. This established the trusted relationship required for the agent to securely propagate identity data from the on premises domain to the cloud.
<p align="center">
  <img src="https://i.imgur.com/4NJ3ti8.png" alt="Image Description" width="700">
</p>

### 2.5 Directory Connection & Synchronisation Configuration

I connected the Microsoft Entra Connect service to my on premises `LAB.local` forest. By creating a dedicated synchronisation account with appropriate directory permissions, I established the necessary bridge to allow user and identity attributes to be read from my local Active Directory for subsequent cloud synchronisation.

> **Configuration Note:** I configured the Entra Connect wizard to "Continue without matching all UPN suffixes," intentionally ignoring the non routable `lab.local` suffix. This action allowed the synchronisation service to proceed, leveraging the previously updated routable `frankminaitlab.onmicrosoft.com` UPNs for all synchronised identities.

| Step 1 | Step 2 |
| :--- | :--- |
| ![Image Description](https://i.imgur.com/ec3KCjS.png) | ![Image Description](https://i.imgur.com/I0mW7Gi.png) |

| Step 3 | Complete |
| :--- | :--- |
| ![Image Description](https://i.imgur.com/rDuJPg1.png) | ![Image Description](https://i.imgur.com/YTBl6ps.png) |

### 2.6 Domain and OU Filtering
I opted to "Sync all domains and OUs" for the initial configuration. This ensures that the entire directory structure is replicated to Microsoft Entra ID, providing a complete view of the lab environment's identity objects in the cloud. This approach maximises visibility while I explore the synchronisation process.
<p align="center">
  <img src="https://i.imgur.com/U1XTpib.png" alt="Image Description" width="700">
</p>

### 2.7 Identity Identification

I configured the synchronisation engine to use "Azure managed source anchors" to link my local users to the cloud.

**Why this is the standard:**
* **Permanent Link:** It uses a permanent ID tag (`mS-DS-ConsistencyGuid`) for each user. This tag never changes, even if I rename a user or move them around.
* **Prevents Errors:** Because the ID tag is permanent, the cloud always knows exactly who the local user is. This prevents identity confusion and sync errors.
* **Set-and-Forget:** By letting Azure handle this automatically, I ensure a stable, long term connection between my lab and the cloud without needing manual maintenance.
<p align="center">
  <img src="https://i.imgur.com/RhuP0bD.png" alt="Image Description" width="700">
</p>

### 2.8 Group-Based Filtering
I configured the sync engine to "Synchronise all users and devices." This ensures my entire lab directory is replicated to the cloud, allowing for a comprehensive and low maintenance test environment.
<p align="center">
  <img src="https://i.imgur.com/hGO3cfi.png" alt="Image Description" width="700">
</p>

### 2.9 Optional Features
I maintained the default selection of "Password hash synchronisation." This is a fundamental component of the hybrid identity model, ensuring that user credentials can be validated in Microsoft Entra ID without requiring additional infrastructure such as a full Exchange hybrid deployment.
<p align="center">
  <img src="https://i.imgur.com/dNWb0wq.png" alt="Image Description" width="700">
</p>

### 2.10 Enabling Single Sign-On (SSO)
I provided domain administrator credentials to configure the on premises forest for seamless single sign-on. This step automates the necessary Kerberos computer account configuration, enabling users to authenticate to cloud services automatically while logged into their domain joined machines, significantly enhancing the user experience.
| Before | After |
| :--- | :--- |
| ![Image Description](https://i.imgur.com/DC2cPCT.png) | ![Image Description](https://i.imgur.com/WBhrwxk.png) |

### 2.11 Final Configuration
I reviewed the final summary to ensure all settings—specifically Single Sign-On and password synchronisation were configured correctly.

I made two key choices to finalise the installation:
* **Start syncing immediately:** I checked this so the service starts working right away, allowing me to verify that my local users are appearing in the cloud.
* **Keep Staging Mode off:** I left this unchecked because this is a single server lab. I want the server to actively sync with the cloud, not just sit in "backup" mode.

Everything looked good, so I selected "Install" to complete the setup and have the synchronisation process running.
|  |  |
| :--- | :--- |
| ![Image Description](https://i.imgur.com/Eupug8X.png) | ![Image Description](https://i.imgur.com/Wu4s4AQ.png) |


### 2.12 Post Installation Verification
Following the successful configuration, I performed the following verification and hardening steps:
* **Service Validation:** Confirmed that the "Microsoft Entra ID Sync" service is active on the local host.
* **Cloud Sync Check:** Verified in the Microsoft Entra admin center that on premises user objects are successfully synchronising.
* **Identity Hardening:** Enabled the Active Directory Recycle Bin to protect against accidental object deletion.
* **SSO Implementation:** Configured Group Policy to assign the SSO endpoint to the Local Intranet zone, ensuring seamless authentication for domain joined clients.

| Service Validation | Cloud Sync Check |
| :--- | :--- |
| ![Image Description](https://i.imgur.com/SADg2kI.png) | ![Image Description](https://i.imgur.com/87EMjkb.png) |

| Identity Hardening | SSO Implementation |
| :--- | :--- |
| ![Image Description](https://i.imgur.com/Eb031sW.png) | ![Image Description](https://i.imgur.com/eOAi6HT.png) |
![Image Description](https://i.imgur.com/EHVlhwc.png)

![Image Description](https://i.imgur.com/87EMjkb.png)
---

## ☁️ Phase 3: Hybrid Identity & SSO Validation
---
**Objective:** Validate that synchronised identities are correctly provisioned and that Hybrid Microsoft Entra ID Join and Seamless SSO configurations provide a seamless authentication experience.

### 3.1 New User Synchronisation
I created a test user account (`Sync Test`) and initiated a manual delta synchronisation.
* **Command:** `Start-ADSyncSyncCycle -PolicyType Delta`
![Image Description](https://i.imgur.com/yc8maEs.png)

### 3.2 End-to-End Single Sign-On SSO Validation
To verify the successful implementation of Hybrid Microsoft Entra ID Join and Seamless Single Sign-On (SSO), the following validation steps were performed:

* **Device Identity**: Validated device trust via dsregcmd /status (Confirmed AzureAdJoined: YES).
* **User SSO Session**: Validated user SSO session via dsregcmd /status (Confirmed AzureAdPrt: YES).
* **Service Access**: Validated service access by navigating to m365.com and confirming automatic sign in without manual credential entry.
* **Result:** The system authenticated automatically using domain credentials without requiring manual password entry.
![Image Description](https://i.imgur.com/vLGY1W1.png)
![Image Description](https://i.imgur.com/5I6ZWyC.png)
---

## 🛠️ Troubleshooting & Operational Resilience

* **Issue:** 'Unsupported browser' error during Entra Connect sign in phase.
* **Resolution:** Configured Microsoft Edge as the default system browser and temporarily adjusted Internet Explorer Enhanced Security Configuration (ESC) to allow the authentication handoff.
* **Takeaway:** This highlighted the necessity of ensuring compatibility between cloud based authentication flows and hardened Windows Server environments.

### 💡 Lessons Learned & Key Takeaways
* **UPN Consistency:** This lab reinforced the critical importance of UPN consistency for synchronisation; ensuring the on premises UPN matches the routable cloud domain is a fundamental prerequisite for identity mapping.
* **GPO-Based User Experience:** I gained practical experience in using Group Policy Objects to enhance the user experience, specifically by automating the configuration of the Local Intranet zone for SSO.
* **Data Hygiene:** Proactive data hygiene such as auditing UPNs and cleaning up system level objects before running a sync agent is essential to prevent synchronisation conflicts and ensures a cleaner, more manageable cloud directory.
* **Environment Hardening:** Working with hardened Windows Server environments highlighted the necessity of managing browser engine compatibility (such as disabling ESC or adjusting default browser settings) to ensure cloud integrated authentication installers function correctly.

### 🛠️ Recommendations for Production Environment
* **Dedicated Sync Server:** In a production environment, isolate the Microsoft Entra Connect Sync service on a dedicated, hardened member server rather than the Domain Controller (DC01) to ensure better resource separation and security posture.
* **Public Domain Integration:** Replace the default `.onmicrosoft.com` domain with a verified, custom public domain to ensure professional branding and simplified end user authentication.
* **Azure AD Connect Health:** Implement Azure AD Connect Health for proactive monitoring of synchronisation performance, connectivity, and potential sync errors, ensuring enterprise grade operational resilience.
* **Automated Audit Tools:** While manual PowerShell scripts are effective for labs, adopt Microsoft’s supported identity management tools and compliance dashboards in production environments to maintain long term alignment with evolving cloud security best practices.

---

## 🏁 Summary of Outcomes
Built a production ready Hybrid Identity bridge, successfully synchronising on premises identities to the cloud. This environment now supports automatic device trust, Seamless SSO, and centralised cloud resource management, serving as a scalable framework for future security and device management initiatives.

## 🚀 Future Roadmap
Future phases will focus on Conditional Access Policies, Multi-Factor Authentication (MFA) enforcement, and Intune device management to further harden the cloud security posture.
