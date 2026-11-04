<h1>
  <img src="../../assets/VKS.png" style="height:30px; vertical-align:middle;"> Supervisor with "NSX + DTGW/VNA"
</h1>

<div class="grid" markdown style="grid-template-columns: 60% 40%">

<div markdown>

This section describes the procedures for **Troubleshooting Network Services into the VKS Namespace utilizing an "NSX + DTGW/VNA" architecture** inside a vSphere environment.

* **Packet Walk**  
    * [N/S External to VIP](2i1-packetwalk-ext_vip.md)  
    * [N/S External to VM](#packetwalk)  
    * [**N/S NAT to External**](#packetwalk)  
    * [E/W Pod to Pod](2i3-packetwalk-pod_pod.md)  
    * [E/W VM to VM](2i4-packetwalk-vm_vm.md)  
* App Access Broken  
    * [VIP Access Down](2j1-troubleshooting-vip.md)  
    * [VM Access Down](2j2-troubleshooting-vm.md)  
    * [Pod Access Down](2j3-troubleshooting-pod.md)  

</div>

<div markdown>
<img src="../images/2f1-0-VM.jpg" style="width: 85% !important; max-width: 250px;" alt="VDS Architecture">
</div>
</div>

---

## Packet Walk - N/S NAT to External {: #packetwalk }

One or a few VMs have been deployed (see [Application Deployment > App Deployment (VMs) > via vCenter UI](2f1-deployment-vms.md#deployment_vms) or [Application Deployment > App Deployment (VMs) > via CLI](2f2-deployment-vms.md#deployment_vms)).

### View

#### Logical View
* **Scenario A: VM connected to Private-VPC Subnet**  
<img src="../images/2i2b-0-LogicalView1.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Logical">

* **Scenario B: VM connected to Private-TGW Subnet**  
<img src="../images/2i2b-0-LogicalView2.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Logical">

#### Physical View
* **Scenario A: VM connected to Private-VPC Subnet**  
<img src="../images/2i2b-0-PhysicalView1.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Physical">

* **Scenario B: VM connected to Private-TGW Subnet**  
<img src="../images/2i2b-0-PhysicalView2.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Physical">

---

### Packet Walk

* **Step1: VM accesses an External server**  
`VM1 (Private-IP) => External-IP`  

    The routing is always done on the source-ESX.  
    **. In case of Private-VPC**  
    The traffic is routed by the VPC Gateway, which sends it to the VPC-SR Active for NAT.  
    **. In case of Private-TGW**  
    The traffic is routed by the VPC Gateway, then the DTGW, then back to the VPC, which sends it to the VPC-SR Active for NAT (in VNA Node).  
    

* **Step2: NAT accesses the External server**  
` NAT (10.1.7.131) => External-IP`  

    **. For both cases Private-VPC and Private-TGW**  
    The VPC-SR Active Source-NAT the VM-IP and sends it back to the VPC Gateway, which routes to the DTGW, and physical fabric.

