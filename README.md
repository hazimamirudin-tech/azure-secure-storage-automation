# Secure Azure Cloud Storage Automation Pipeline

An enterprise-grade, zero-trust cloud data pipeline that automates mock retail transaction uploads into a fully isolated Azure environment. This project demonstrates advanced skills in secure networking, identity governance, firewalls, and cloud automation.

## 🏗️ Architecture & Security Design
This implementation follows strict security best practices to protect data at rest and in transit:
* **Private Network Topology:** A multi-subnet Virtual Network (VNet) hosting core assets with Azure Bastion providing passwordless, secure administrative access.
* **Network Isolation:** Public ingress is completely disabled. Traffic travels through a Private Endpoint mapped to a dedicated Private DNS Zone (`privatelink.blob.core.windows.net`).
* **Zero-Trust Access Control:** Enforces Microsoft Entra ID Role-Based Access Control (RBAC). A System-Assigned Managed Identity is used to authorise compute access without managing connection strings.
* **Stateful Firewalls:** Granular Network Security Groups (NSGs) restricting storage access solely to trusted subnets over encrypted HTTPS (Port 443).
* **Data Lifecycle Automation:** Custom compliance governance rules configured to automatically delete hot test objects after 3 days for strict cost optimisation.

## 🚀 Automated Data Pipeline (GitHub Actions)
The system leverages external CI/CD automation runners to simulate scheduled mock transaction events across a dynamic daily production timeline:
1. **Runner Ingress:** GitHub Actions runners dynamically fetch their execution IP.
2. **Ephemeral Firewall Exception:** The runner temporarily whitelists its host IP to cross the secure Azure storage perimeter.
3. **Data Injection:** Evaluates current operational hour metrics to upload targeted bread distribution records:
   * **10:00 AM (MYT):** Injects 2 Sourdough items.
   * **03:00 PM (MYT):** Injects 3 Cheese items.
   * **Manual Ingestion:** Generates 5 baseline White Bread logs instantly.
4. **Perimeter Lockdown:** The automation script strips the firewall exception rule upon completion, restoring absolute isolation.

## 🔍 Verification & Compliance Proof
* **Identity Validation:** User A (Authorised Data Contributor) possesses proper Entra ID entitlements to query data blocks via Azure Bastion VM terminal sessions.
* **Intruder Defence:** User B (Blocked Identity) encounters hard `Access Denied` validation warnings upon data navigation attempts.
