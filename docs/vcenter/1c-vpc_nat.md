<h1>
  <img src="../../assets/vCenter.png" style="height:30px; vertical-align:middle;"> VPC NAT Configuration in vCenter
</h1>

<div class="grid" markdown style="grid-template-columns: 80% 20%">

<div markdown>

This section describes the procedures for configuring Network Address Translation (NAT) for your VPC workloads in private subnets.
<br><br>
**NAT** allows VPC Private subnets to communicate with external networks or provides a way for external users to reach private VPC workloads.
</div>

<div markdown>
<img src="../images/1c-0-VPC_NAT.jpg" style="width: 85% !important; max-width: 150px;" alt="vCenter VPC Connectivity">
</div>

</div>

---

## Overview of NAT types

Different NAT types are available:

| Type | Use Case | Routing Logic |
| :--- | :--- | :--- |
| [**External-IP<br>(1:1)**](#ext-ip) | Maps a single Public IP to a single Private IP for specific workloads. | Provides Bi-directional communication (Inbound/Outbound) for Private VPC and TGW workloads. |
| [**Outbound-NAT<br>(N:1 SNAT)**](#outbound-nat)| Allows multiple workloads to share a single Public IP for external access. | Supports outbound requests only; hides internal IP addresses from the physical network. |
| [**NAT<br>(SNAT/DNAT)**](#full-nat) | Provides stateful Layer 4 translation for granular traffic control. | Enables flexible port-level translation and mapping between Private subnets and external networks. |

<img src="../images/1c-0-VPC_NAT_types.jpg" style="display: block; margin: 0 auto; width: 95% !important;" alt="VPC NAT">

---

## External-IP (1:1 NAT) {: #ext-ip }

Maps a single Public IP to a single Private IP for specific workloads.

<img src="../images/1c-1-VPC_NAT_ExtIP.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 600px;" alt="VPC NAT Ext-IP">

### Configuration

* **Step1. Create a new External IP**
    <img src="../images/1c-1a-Create_VPC_NAT_ExtIP.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="vCenter Create External IP">

### Monitoring
* ***Show External IPs**
    <img src="../images/1c-1b-Validation_VPC_NAT_ExtIP.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="vCenter Result VPC Subnet">

---
## Outbound-NAT (N:1 SNAT) {: #outbound-nat }

Allows multiple workloads to share a single Public IP for external access.

<img src="../images/1c-2-VPC_ONAT.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 600px;" alt="VPC NAT Ext-IP">

### Configuration
<div style="margin-left: 40px; margin-right: 40px;" markdown="1">
!!! warning "Limitation"
    Outbound-NAT is **not supported** on VPCs connected to Distributed Transit Gateways without VNA.  
    (only supported on VPCs connected to Centralized Transit Gateways or Distributed Transit Gateways with VNA)
</div>

* ***Step1. Check Outbound-NAT configuration in the VPC Gateway**
    <img src="../images/1c-2a-Check_VPC_ONAT_Config.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="vCenter Check Outbound NAT Config">

* **Step2. If disabled, Edit the VPC to find the Connectivity Profile used by the VPC**
    <img src="../images/1c-2b-Find_Connectivity_Profile.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 800px;" alt="vCenter Validation VPC Subnet">

* **Step3. Edit the Connectivity Profile and enable Outbound-NAT**
    <img src="../images/1c-2c-Enable_ONAT.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="vCenter Validation VPC Subnet">

### Monitoring
* **Show the Outbound-NAT IP used by the VPC**
    <img src="../images/1c-2d-Validation_VPC_ONAT.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="vCenter Result VPC Subnet">

---

## NAT (SNAT/DNAT) {: #full-nat }

Provides stateful Layer 4 translation for granular traffic control.

xxx To do...

### Configuration

xxx To do...

### Monitoring

xxx To do...

---

