<h1>
  <img src="../../assets/VKS.png" style="height:30px; vertical-align:middle;"> Supervisor with "VDS + FLB"
</h1>

<div class="grid" markdown style="grid-template-columns: 60% 40%">

<div markdown>

This section describes the procedures for **Troubleshooting Network Services into the VKS Namespace utilizing a "VDS + FLB" architecture"** inside a vSphere environment.

* **Packet Walk**  
    * [N/S External to VIP](1h1-packetwalk-ext_vip.md)  
    * [**N/S External to VM**](#packetwalk)  
    * [E/W Pod to Pod](1h3-packetwalk-pod_pod.md)  
    * [E/W VM to VM](1h4-packetwalk-vm_vm.md)  

</div>

<div markdown>
<img src="../images/1e2-0-VM.jpg" style="width: 85% !important; max-width: 250px;" alt="VDS Architecture">
</div>
</div>

---

## Packet Walk - N/S External to VM {: #packetwalk }

One or a few VMs have been deployed (see [Application Deployment > App Deployment (VMs) > via CLI](1e2-deployment-vms.md#deployment_vms)).

### View

#### Logical View
<img src="../images/1h2-0-LogicalView.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Logical">

#### Physical View
* **Use case: VM connected to Public Subnet**  
<img src="../images/1h2-0-PhysicalView.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Physical">

---

### Packet Walk

* **Step1: External Client accesses the VM**  
`Client-IP => VM1 (10.1.11.141)`  

    The traffic enters the ESX hosting the VM.

