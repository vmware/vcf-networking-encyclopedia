<h1>
  <img src="../../assets/VKS.png" style="height:30px; vertical-align:middle;"> Supervisor with "NSX + CTGW/Edge/T0"
</h1>

<div class="grid" markdown style="grid-template-columns: 60% 40%">

<div markdown>

This section describes the procedures for **Troubleshooting Network Services into the VKS Namespace utilizing an "NSX + CTGW/Edge/T0" architecture** inside a vSphere environment.

* **Packet Walk**  
    * [N/S External to VIP](3i1-packetwalk-ext_vip.md)  
    * [N/S External to VM](3i2-packetwalk-ext_vm.md)  
    * [**N/S NAT to External**](#packetwalk) 
    * [E/W Pod to Pod](3i3-packetwalk-pod_pod.md)  
    * [E/W VM to VM](3i4-packetwalk-vm_vm.md)  
* App Access Broken  
    * [VIP Access Down](3j1-troubleshooting-vip.md)  
    * [VM Access Down](3j2-troubleshooting-vm.md)  
    * [Pod Access Down](3j3-troubleshooting-pod.md)  

</div>

<div markdown>
<img src="../images/3f1-0-VM.jpg" style="width: 85% !important; max-width: 250px;" alt="VDS Architecture">
</div>
</div>

---

## Packet Walk - N/S External to VM {: #packetwalk }

One or a few VMs have been deployed (see [Application Deployment > App Deployment (VMs) > via vCenter UI](3f1-deployment-vms.md#deployment_vms) or [Application Deployment > App Deployment (VMs) > via CLI](3f2-deployment-vms.md#deployment_vms)).

### View

#### Logical View
* **Scenario A: VM connected to Private-VPC Subnet**  
<img src="../images/3i2b-0-LogicalView1.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Logical">

* **Scenario B: VM connected to Private-TGW Subnet**  
<img src="../images/3i2b-0-LogicalView2.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Logical">

#### Physical View
* **Scenario A: VM connected to Private-VPC Subnet**  
<img src="../images/3i2b-0-PhysicalView1.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Physical">

* **Scenario B: VM connected to Private-TGW Subnet**  
<img src="../images/3i2b-0-PhysicalView2.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Physical">

---

### Packet Walk

* **Step1: VM accesses an External server**  
`VM1 (Private-IP) => External-IP`  

    The VM sends the packet to its default gateway (VPC-DR) hosted on its local ESX.  
    VPC-DR routes to its default gateway: VPC-SR.  
    *Note: The Active VPC-SR hosted on an Edge Nodes could be on a different ESX (as represented).  
    The traffic passing from the ESX host to the Edge Node is encapsulated between the ESX TEP and the Edge TEP.*
    
    **. In case of Private-VPC**  
    Because Private-VPC subnet traffic leaving the VPC requires Source NAT (SNAT), the traffic is Source-NATed by the VPC Gateway.  
    **. In case of Private-TGW**  
    The traffic is routed by the VPC Gateway, then the CTGW.
    Because Private-TGW subnet traffic leaving the CTGW requires Source NAT (SNAT), the traffic is Source-NATed by the CTGW.  
    *Note: The Active VPC-SR and CTGW could reside on different Edge Nodes. In such cases, cross-Edge Node traffic would occur (not represented).*  

* **Step2: SNAT routes the traffic up to the T0**  
` NAT (10.1.7.131) => External-IP`  

    **. In case of Private-VPC**  
    VPC-SR routes to the default gateway: CTGW-Active.  
    CTGW routes to the default gateway: T0-Active.  
    **. In case of Private-TGW**  
    CTGW routes to the default gateway: T0-Active.  

    *Note: The Active CTGW and T0 could reside on different Edge Nodes. In such cases, cross-Edge Node traffic would occur (not represented).  
    Also, in an Active/Active T0 configuration, CTGW traffic would be forwarded across all Edge Nodes hosting the T0 (not represented).*

* **Step3: T0 routes to the physical fabric**  
` NAT (10.1.7.131) => External-IP`  

    T0-Active routes to its default gateway: Physical Router.  
    *Note: In the case of multiple physical routers configured in Active/Active (not represented), the T0 routes traffic among all available physical routers.*


!!! info "Architectural Note: Asymmetric Routing"
    The North/South (not represented) and South/North traffic can be asymetric: processed by one physical router inbound, and a different physical router outbound.

    !!! Warning "Stateless Physical Routing Required"
        Because the traffic path can be asymmetric, the upstream physical network devices must operate as standard routers, not stateful firewalls.  
        A stateful firewall will flag asymmetric return traffic as invalid and drop the packets.  
        If the T0 routes traffic to a firewall, configure the T0 as Active/Standby and configure BGP so 1 path is prefered to avoid asymetric traffic.
