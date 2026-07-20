## Copilot instructions for StorageGRID appliances documentation

### Repository overview
Product: NetApp StorageGRID appliances

NetApp StorageGRID appliances are purpose-built hardware platforms that run as nodes in a StorageGRID system—a software-defined object storage suite with native Amazon S3 API support. This repository documents the installation, configuration, and maintenance of all StorageGRID appliance hardware.

### Repository structure
- `installconfig/` – Hardware overviews for all appliance models, plus installation and initial configuration procedures (cabling, network setup, StorageGRID Appliance Installer, BMC, SANtricity, node deployment)
- `commonhardware/` – Maintenance tasks common to all appliances: maintenance mode, node cloning, BMC usage, DNS/MTU configuration, node encryption
- `_include/` – Shared AsciiDoc content fragments included by reference across multiple pages (hardware-specific reuse snippets, FRU statements, LED descriptions)
- `sg100-1000/` – Maintenance procedures specific to SG100 and SG1000 services appliances
- `sg110-1100/` – Maintenance procedures specific to SG110 and SG1100 services appliances
- `sg120-1200/` – Maintenance procedures specific to SG120 and SG1200 services appliances
- `sg5600/` – Maintenance procedures for SG5600 series storage appliances
- `sg5700/` – Maintenance procedures specific to SG5700 series storage appliances
- `sg5800/` – Maintenance procedures specific to SG5800 series storage appliances
- `sg6000/` – Maintenance procedures specific to SG6000 series storage appliances (SG6060, SG6060X, SGF6024)
- `sg6100/` – Maintenance procedures specific to SG6100 series storage appliances (SGF6112, SG6160)
- `sg6200/` – Maintenance procedures specific to SG6200 series storage appliances (SGF6212, SG6260)
- `landing-learn-hdwr/`, `landing-install-prep/`, `landing-install-hdwr/`, `landing-appl-install/`, `landing-deploy-appliance/`, `landing-maintain-hdwr/` – Section landing pages for top-level navigation groups
- `landing-maintain-sg1000-100/`, `landing-maintain-sg1100-110/`, `landing-maintain-sg1200-120/`, `landing-maintain-sg5700/`, `landing-maintain-sg5800/`, `landing-maintain-sg6000/`, `landing-maintain-sg6200/`, `landing-maintain-sgf6112/` – Per-model maintenance landing pages
- `redirect/` – URL redirect mappings for the publishing system
- `media/` – Images referenced throughout the documentation

### Product-specific context

**Architecture and components:**
- A *StorageGRID grid* is composed of nodes; appliances run as hardware-based nodes alongside software-only (virtual) nodes in the same deployment
- *Services appliances* (SG100, SG110, SG120, SG1000, SG1100, SG1200) run as Admin Nodes or Gateway Nodes and handle grid administration and load balancing; they do not store object data
- *Storage appliances* (SG5700, SG5800, SG6000, SG6100, SG6200 series) run as Storage Nodes and store object data
- Storage appliances with separate compute and storage controllers connect those controllers via Fibre Channel (SG6000 series) or internal interconnect (SG5700, SG5800 series)
- All appliances connect to up to three StorageGRID networks: *Grid Network* (required), *Admin Network* (optional), *Client Network* (optional)

**Key concepts:**
- *StorageGRID Appliance Installer* – A browser-based tool resident on each appliance used to configure network connections and deploy the appliance as a StorageGRID node; accessible at port 8443
- *BMC (Baseboard Management Controller)* – A low-level hardware management interface present on all appliances; used for monitoring, alerts, power control, and SNMP configuration
- *SANtricity System Manager* – A browser-based management tool for E-Series storage controllers; used on storage appliances that include E-Series controllers (SG5700, SG5800, SG6000, SG6100, SG6200 series)
- *SANtricity OS* – The firmware running on E-Series storage controllers; managed via SANtricity System Manager or Grid Manager
- *Maintenance mode* – A special operating state where an appliance runs the StorageGRID Appliance Installer instead of StorageGRID, used for hardware maintenance and upgrades
- *Node cloning* – A procedure that copies all data and configuration from one appliance node to a replacement appliance of compatible type
- *High availability (HA) group* – A set of services appliance nodes configured to provide failover using virtual IP addresses
- *ILM (Information Lifecycle Management)* – The StorageGRID policy engine that governs how object copies are stored and protected; referenced in appliance context for data protection behavior
- *DDP / DDP16* – Dynamic Disk Pool configurations available on SG6000-series storage shelves that improve rebuild performance compared to standard RAID 6

**Naming conventions and terminology:**
- *SG* prefix denotes StorageGRID appliances (e.g., SG100, SG5800, SG6160)
- *SGF* prefix denotes all-flash StorageGRID appliances (SGF6024, SGF6112, SGF6212)
- Services appliance models pair a lower-bandwidth and higher-bandwidth unit: SG100/SG1000, SG110/SG1100, SG120/SG1200; within a pair, the four-digit model (e.g., SG1000) has higher network throughput
- *E5700SG* – The compute controller used in SG5700 appliances (distinguished from the E5700 storage controller)
- *SG6000-CN* – The compute controller in SG6000 series appliances
- *SG6100-CN* – The compute controller in SG6100 and SG6200 series appliances
- Storage controller generations used across appliance families: *E2800* (SG5700, SG6000), *E4000* (SG5800, SG6100, SG6200), *EF570* (SGF6024)
- "X" suffix models (e.g., SG5712X, SG6060X) differ from base models only in interconnect port location on the storage controller; do not mix A and B (or X and non-X) storage controllers in the same appliance
- *FRU (Field Replaceable Unit)* – Components that can be replaced on-site; the term is used in maintenance procedures
- *Expansion shelf* – An additional drive shelf (DE460C or DE212C enclosure) that can be added to SG6060, SG6160, and SG6260 appliances to increase drive capacity

### Typical user workflows

**Initial hardware installation:** Unpack and rack appliance → Cable to StorageGRID networks → Apply power → Access StorageGRID Appliance Installer → Configure network links and IP addresses → Register hardware → Deploy as StorageGRID node

**Configure storage appliance hardware:** Access Appliance Installer → Configure network connections → Configure BMC interface → (Optional) Configure SANtricity System Manager → (Optional) Enable node encryption or change RAID mode → Deploy Storage Node

**Perform appliance maintenance:** Place appliance into maintenance mode via Grid Manager → Access Appliance Installer for maintenance tasks → Perform FRU replacement or upgrade → Reboot into StorageGRID to exit maintenance mode

**Upgrade SANtricity OS on storage controllers:** Download new SANtricity OS → Apply via Grid Manager (online method) or via SANtricity System Manager in maintenance mode (offline method) → Verify upgrade on all storage controllers

**Appliance node cloning:** Install replacement appliance alongside source → Cable replacement to same networks → Access Appliance Installer on replacement → Start clone operation → Monitor progress → Decommission source appliance after clone completes
