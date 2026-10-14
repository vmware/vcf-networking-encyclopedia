<h1>
  <img src="../../assets/VKS.png" style="height:30px; vertical-align:middle;"> Supervisor with "VDS + FLB"
</h1>

<div class="grid" markdown style="grid-template-columns: 60% 40%">

<div markdown>

This section describes the procedures for **deploying the Supervisor utilizing a "VDS + FLB" architecture** inside a vSphere environment.

* **Supervisor**
    * [**Deployment**](#supervisordeployment)
    * [Access via CLI](1b2-access-supervisor.md)
</div>

<div markdown>
<img src="../images/0-VDS.jpg" style="width: 85% !important; max-width: 250px;" alt="VDS Architecture">
</div>
</div>

---

## Supervisor Deployment {: #supervisordeployment }

<img src="../images/1b-0-Topology.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Topology">

### Create Supervisor
Navigate to **vCenter** > **Supervisor Management**, and click **Get Started**.
<img src="../images/1b-1-AddSupervisor.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 800px;" alt="Add Supervisor Wizard">

1. **vCenter Server and Network**  
    * Select the network stack **vSphere Distributed Switch (VDS) with Load Balancer Type "Foundation Load Balancer"**, and click **Next**.  

2. **Supervisor Location**  
    * Select the **Cluster Deployment**, and click **Next**.  
    <img src="../images/1b-1b-Supervisor.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Supervisor Location Settings">  

3. **Storage**  
    * Select the different **Storage Policies**, and click **Next**.  
    <img src="../images/1b-1c-Storage.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Storage Policy Selection">  

4. **Management Network**  
    * Configure the **Supervisor Management IP Settings**, and click **Next**.  
    <img src="../images/1b-1d-Management.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Management Network IP Settings">  

5. **Workload Network**  
    * Configure the **Workload Network**, and click **Next**.  
        Choose the **Port Group** to host the K8s Nodes and configure the **Layer 3 Routing Configuration** for the Supervisor and future K8s Nodes and VMs.  
        
        > **Note:** The Internal Network for Kubernetes Services is pre-populated with the subnet `10.96.0.0/23`.  

        <img src="../images/1b-1e-Workload.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Workload Network Configuration">  

6. **Load Balancer**  
    * Configure the **Foundation Load Balancer**, and click **Next**.  
    Choose its **Network Topology** (One-Arm or Two-Arm), its **Size**, **High-Availability** architecture (1 FLB Active, or 2 FLB Active/Standby VMs), and configure the following networks:  
        * **Management Network** (see picture "1" below)  
        Choose the **Port Group** to host the FLB Node(s) Management vNIC and specify its Layer 3 routing configuration.
        * **Virtual Server Network** (see picture "2" below)  
        Choose the **Port Group** to host the FLB Node(s) VIP vNIC and specify its Layer 3 routing configuration.
        * **Transit Network**
        By default, the same Port Group used by the Supervisor and future K8s Nodes/VMs is selected, but this can be changed. This is the FLB(s) vNIC used to communicate with the Supervisor and workloads.

    <img src="../images/1b-1f-LoadBalancer.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Advanced Settings and Sizing">  
    <img src="../images/1b-1f1-LoadBalancer.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 600px;" alt="Advanced Settings and Sizing">  
    <img src="../images/1b-1f2-LoadBalancer.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 600px;" alt="Advanced Settings and Sizing">  

7. **Advanced Settings**  
    * Select the **Supervisor Control Plane Size**, and click **Next**.  
    <img src="../images/1b-1g-Advanced.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Advanced Settings and Sizing">  

8. **Ready to Complete**  
    * Review your configuration and click **Finish**.  
    <img src="../images/1b-1h-Ready.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Review and Complete">  


---

### Finish Supervisor Creation for future Kubernetes Clusters

#### **Create a Content Library for future Kubernetes Clusters**  
If you plan to deploy Kubernetes Clusters, create a Content Library with VKS images.  
Navigate to **vCenter** > **Content Libraries** > and click **Create**.  
    <img src="../images/2c1-2a-ContentLibrary.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="vCenter Server and Network Configuration">  

  1. **Name and Location**  
    Give it a **Name** and select the **vCenter** hosting that Content Library, and click **Next**.  
    <img src="../images/2c1-3a-Name.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 800px;" alt="vCenter Server and Network Configuration">  

  2. **Configure Content Library**  
    Choose between **Local content library** (you manually upload VKS images) and **Subscribed content library** (vCenter automatically downloads VKS images from a repository), and click **Next**.  
    > *Note: This example uses a Subscribed content library pointing to the public repository `https://wp-content.vmware.com/v2/latest/lib.json`, with the option to "download content when needed" to save local storage space.*  

    <img src="../images/2c1-3b-ContentLibrary.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 800px;" alt="vCenter Server and Network Configuration">  

  3. **Apply Security Policy**  
    Apply a **security policy** if required by your organization, and click **Next**.  
    *(This example uses no security policy).*  
    <img src="../images/2c1-3c-SecurityPolicy.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 800px;" alt="vCenter Server and Network Configuration">  

  4. **Add Storage**  
    Select a **storage** to host the content library images, and click **Next**.  
    <img src="../images/2c1-3d-Storage.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 800px;" alt="vCenter Server and Network Configuration">  

  5. **Ready to complete**  
    Review the content library, and click **Finish**.  
    <img src="../images/2c1-3e-Review.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 800px;" alt="vCenter Server and Network Configuration">  


#### **Associate the Content Library to the Supervisor**  
Navigate to **vCenter** > **Supervisor Management** > **Supervisors**, select **[your supervisor]**, and click on **Kubernetes Service - Manage**.
    <img src="../images/1b-4a-Supervisor.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="vCenter Server and Network Configuration">  

  1. **Add Content Library**  
    Click **ADD**.  
    <img src="../images/2c1-4b-AddContentLibrary.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="vCenter Server and Network Configuration">  

  2. **Select Content Library**  
    Select **Content Library with VKS images**, and click **ADD**.  
    <img src="../images/2c1-4c-SelectContentLibrary.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 600px;" alt="vCenter Server and Network Configuration">  

---

### Validate Deployment

#### **Validate Supervisor Status**  
Once the wizard completes, verify the deployment was successful by navigating to **vCenter** > **Supervisor Management** > **Supervisors**. 

Check the following fields to ensure they reflect a healthy state:
<ul style="margin-top: -10px; margin-bottom: 15px; line-height: 1.3;">
  <li style="margin-bottom: 2px;">Config Status</li>
  <li style="margin-bottom: 2px;">Host Config Status</li>
  <li style="margin-bottom: 2px;">Control Plane Node Address</li>
</ul>
<img src="../images/1b-5a-Validation.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 800px;" alt="Supervisor Validation Status">

#### **Validate Supervisor Content Library**  
Validate Supervisor Content Library by navigating to **vCenter** > **Supervisor Management** > **Supervisors**, select **[your supervisor]**
<img src="../images/1b-5b-Validation.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Supervisor Validation Status">

??? info "Ignore Kubernetes Status Warning"
    <img src="../images/1b-5b1-Warning.jpg" style="display: block; margin: 0 auto; width: 75% !important ; max-width: 800px;" alt="Supervisor Warning">
    A Supervisor utilizing the "VDS + FLB" architecture supports a maximum of 2 FLB VMs, meaning it only requires 2 IP addresses within the Management and Virtual Server Networks (configured during Step 6).  
    If exactly 2 IPs are allocated to those networks, the pool is fully consumed immediately upon deployment, triggering a low-IP warning in the UI.  
    **This warning is expected behavior in this specific architecture and can be safely ignored.**