<h1>
  <img src="../../assets/VKS.png" style="height:30px; vertical-align:middle;"> Supervisor with "NSX + DTGW/VNA"
</h1>

<div class="grid" markdown style="grid-template-columns: 60% 40%">

<div markdown>

This section describes the procedures for **provisioning and managing Network Services within a VKS Namespace utilizing an "NSX + DTGW/VNA" architecture** inside a vSphere environment.

* **Network Services**
    * [**Subnets**](#networkservices)
    * [SubnetSets](2h2-network-subnetset.md)
    * [Static Routes](2h3-network-staticroute.md)
    * [External IPs](2h4-network-externalip.md)
    * [VM Load Balancers](2h5-network-lb.md)

</div>

<div markdown>
<img src="../images/2h1-0-subnet.jpg" style="width: 85% !important; max-width: 250px;" alt="VDS Architecture">
</div>
</div>

---

## Network Services - Subnets {: #networkservices }

<img src="../images/2h1-1-Topology.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Topology">

### Create Subnet

Navigate to **vCenter** > **Supervisor Management** > **Supervisors**, select your target Supervisor, click the **Namespaces** tab, and select your specific Namespace.  
Under the **Resources** card, click **Network - Go to Service**.  
<img src="../images/2h1-1-network.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Network Service">

1. **Create New Subnet**  
Navigate to **Subnets**, and click **New Subnet**.  
<img src="../images/2h1-1a-subnetcreate.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Create Subnet">  

For more information about Subnets settings review the [VPC Subnet Overlay page](../vcenter/1b-vpc_subnet.md#overlay){target="_blank"}.


---

### Validate Subnet Creation
Expand the newly created Subnet to view its assigned CIDR and VPC Gateway.
<img src="../images/2h1-2a-subnetcreated.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Created Subnet">  

