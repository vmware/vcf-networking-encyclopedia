<h1>
  <img src="../../assets/vCenter.png" style="height:30px; vertical-align:middle;"> VNA Configuration in vCenter
</h1>

<div class="grid" markdown style="grid-template-columns: 70% 30%">

<div markdown>

This section describes the procedures for configuring NSX Virtual Network Appliances (VNA) using the vSphere Client.
<br><br>
**NSX Virtual Network Appliances (VNA Nodes)** provide the centralized network services required **for Distributed Transit Gateway (DTGW)** designs within the VPC architecture.
</div>


<div markdown>
<img src="../images/2b-0-VNA.jpg" style="width: 85% !important; max-width: 250px;" alt="vCenter VNA">
</div>

</div>

---

## VNA Cluster / VNA Nodes

### Configuration

* **Step1. Create new VNA Cluster / VNA Nodes**
    <img src="../images/2b-1-Create_VNA.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="vCenter VNA Cluster">

* **Step2. Configure VNA Cluster**
    <img src="../images/2b-2-Create_VNA.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 600px;" alt="vCenter VNA Cluster config">

    * **Node Form Factor**:  
      Select the appliance size (vCPU / Memory) of the VNA Nodes.  
      This determines the scale and performance limits for the logical routers (VPC-SR Gateways) hosted on the node.

* **Step3. Configure VNA Nodes Placement and Networking**
    <img src="../images/2b-3a-Configure_VNANode1.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="vCenter VNA Node">

    * **vSphere Cluster / Resource Pool / Host Group Affinity / Data Store**:  
      Defines the physical placement of the Edge Node VM.  
      You can optionally specify Resource Pools and Host Group Affinity to control where the VM resides within the cluster.  

    * **Management Network Settings (IP Address Type / IP Assignment / Port Group)**  
      Configures the IP assignment and Port Group for the VNA Node management interface.

**Repeat these steps for the remaining VNA Nodes (2+ nodes recommended).**

<div style="margin-left: 40px; margin-right: 40px;" markdown="1">
??? info "Cloning option"
    In case other Edge Nodes have the vSphere Cluster and Uplink settings, use the cloning option.
    <img src="../images/2b-3b-Configure_VNANode2.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Add Edge Node2">
</div>


### Monitoring
* **Status**
    The status reflects the successful application of the configuration.
    <img src="../images/2b-4-Validation_VNA.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="vCenter VNA Status">

---
