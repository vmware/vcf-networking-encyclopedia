<h1>
  <img src="../assets/VKS.png" style="height:30px; vertical-align:middle;"> Supervisor Network Services
</h1>

This section describes the procedures for **deploying the Supervisor** within a vSphere environment.

---

## Different network options for Supervisor deployment

### 1. Without NSX

**Architecture:** Uses VDS port groups and deploys or consume load balancers to handle load balancing traffic.  
**Best for:** VMware vSphere Foundation (VVF), lab environments, Proof of Concepts (PoC), or smaller environments.  
**Limitation:** Limited Network Services (only LB) and limited VCF Automation support (no namespace create/delete/update from VCFA).

* **1a. VDS + FLB**  

| <div style="width: 100px;">Metric /Feature</div> | <div style="min-width: 140px;">**1a. VDS + FLB**<br><img src="images/0-VDS.jpg" style="width: 85% !important; max-width: 200px;" alt="VDS Architecture">|
| :--- | :--- |
| **Physical Fabric** | MTU ≥ 1500,<br> L2 Physical |
| **VCF-A Support** | 🔴 No |
| **Network Services** | 🔴 Limited <br>(No Subnets, No NAT, No VPN) |
| **Scale** | 🔴 Limited <br>(All VMs and K8s consume a Public IP, <br>all VIPs are managed by a single FLB A/S) |
| **Security** | 🔴 Direct Access <br>(K8s Nodes reachable from external) |
| **High Availability** | 🔴 Limited <br>(Supervisor VMs cross Clusters requires shared-VDS) |

---

### 2. With NSX

**Architecture:** Uses NSX Distributed or Centralized Transit Gateways and VNA or Edge Cluster to handle load balancing traffic.  
**Best for:** Fully integrated VCF architecture for better scale and security.  
**Consideration:** Requires fully deployed NSX overlay infrastructure with DTGW+VNA or CTGW+Edge+T0.

| <div style="width: 100px;">Metric /Feature</div> | <div style="min-width: 140px;">**2a. NSX +<br>CTGW(TEP)**<br><img src="images/0-NSX_CTGW.jpg" style="width: 85% ; max-width: 200px !important;" alt="NSX Architecture"></div> | <div style="min-width: 140px;">**2b. NSX +<br>DTGW VLAN(TEP)**<br><img src="images/0-NSX_DTGW_VLAN.jpg" style="width: 85% ; max-width: 200px !important;" alt="NSX Architecture"></div> | <div style="min-width: 140px;">**2c. NSX +<br>DTGW VXLAN(TEP)**<br><img src="images/0-NSX_DTGW_VXLAN.jpg" style="width: 85%; max-width: 200px !important;" alt="NSX Architecture"></div> | <div style="min-width: 140px;">**2d. NSX +<br>DTGW VLAN(TEP-Less)**<br><img src="images/0-NSX_DTGW_VLAN_TEP-Less.jpg" style="width: 85%; max-width: 200px !important;" alt="NSX Architecture"></div> |
| :--- | :--- | :--- | :--- | :--- |
| **Physical Fabric** | MTU ≥ 1700,<br> L2 or L3 Fabric with VLAN <br>+ BGP | MTU ≥ 1700,<br> L2 Fabric with VLAN | MTU ≥ 1700,<br> EVPN Fabric <br>+ BGP | MTU ≥ 1500,<br> L2 Fabric with VLAN |
| **VCF-A Support** | 🟢 Yes | 🟢 Yes | 🟢 Yes | 🟢 Yes |
| **Network Services** | 🟢 All | 🟢 Most <br>(All but VPN) | 🟢 Most <br>(All but VPN) | 🔴 Limited <br>(No Subnets private, No NAT, No VPN, Supports VMs with IP unicast traffic only (no multicast/VRRP/HSRP/etc)) |
| **Scale** | 🟢 Large | 🟢 Large | 🟢 Large | 🔴 Limited <br>(All VMs and K8s consume a Public IP)|
| **Security** | 🟢 Isolated | 🟢 Isolated | 🟢 Isolated | 🔴 Direct Access <br>(K8s Nodes reachable from external) |
| **High Availability** | 🟢 Yes | 🟢 Yes | 🟢 Yes | 🟢 Yes |

---

??? note "Upcoming Architectures"
    The following architectures will be covered in future updates:  

    * Avi Load Balancer  

---

!!! info "Document Versioning"
    This guide is updated for **VCF 9.1+**.  
    If you are running an older version, some options may not be available.

---