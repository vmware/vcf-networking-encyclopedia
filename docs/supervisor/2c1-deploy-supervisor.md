<h1>
  <img src="../../assets/VKS.png" style="height:30px; vertical-align:middle;"> Supervisor with "NSX + DTGW/VNA"
</h1>

<div class="grid" markdown style="grid-template-columns: 60% 40%">

<div markdown>

This section describes the procedures for **deploying a Supervisor utilizing an "NSX + DTGW/VNA" architecture** inside a vSphere environment.

* **Supervisor**
    * [**Deployment**](#supervisordeployment)
    * [Access via CLI](2c2-access-supervisor.md)
</div>

<div markdown>
<img src="../images/0-DTGW.jpg" style="width: 85% !important; max-width: 250px;" alt="DTGW Architecture">
</div>
</div>

---

## Supervisor Deployment {: #supervisordeployment }

!!! info "Supervisor Tool App"
    As an option, you can go through the Supervisor deployment with the **Supervisor Tool App web application** that guides you through:  

    - **checking prerequisites** (and adding them if missing)
    - **deploying vSphere Supervisor** in a friendly workflow ⬅️ ⬅️ ⬅️  
    - **validating Supervisor and VKS** environments  
    
    [GitHub Repository](https://github.com/ddesmidt/Supervisor_Install_Tool){:target="_blank"}: Product overview, installation guides (git pull or direct OVA), and recorded demos for all three use cases.

    
    Otherwise, you can also follow the steps below.

<img src="../images/2c1-0-Topology.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Topology">

### Create Supervisor
Navigate to **vCenter** > **Supervisor Management**, and click **Get Started**.
<img src="../images/2c1-1-AddSupervisor.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 800px;" alt="Add Supervisor Wizard">

1. **vCenter Server and Network**  
    * Select the network stack **VCF Networking with VPC (recommended)**, and click **Next**.  
    <img src="../images/2c1-1a-vCenter.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="vCenter Server and Network Configuration">  

2. **Supervisor Location**  
    * Select the **Cluster Deployment**, and click **Next**.  
    Note: You can deploy Supervisor VMs:  
        . High Availability: With HA enabled (3 VMs) or in standalone (non-HA) mode  
        . Placement: Target a vSphere Cluster (shown below) or a vSphere Zone.
    <img src="../images/2c1-1b-Supervisor.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Supervisor Location Settings">  

3. **Storage**  
    * Select the different **Storage Policies**, and click **Next**.  
    <img src="../images/2c1-1c-Storage.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Storage Policy Selection">  

4. **Management Network**  
    * Configure the **Supervisor Management IP Settings**, and click **Next**.  
    <img src="../images/2c1-1d-Management.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Management Network IP Settings">  

5. **Workload Network**  
    * Configure the **Workload Network** (fields are pre-populated, except for DNS and NTP Servers), and click **Next**.  
    <img src="../images/2c1-1e-Workload.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Workload Network Configuration">  

        ??? warning "Troubleshooting: Auto SNAT Error"
            If you receive the error *"Auto SNAT must be enabled for VPC Connectivity Profile Default"*, refer back to the **["DTGW + VNA" Requirements](2a-requirements.md#nsx)** page and ensure **Default Outbound NAT** is enabled in the Connectivity Profile.

6. **Advanced Settings**  
    * Select the **Supervisor Control Plane Size**, and click **Next**.  
    <img src="../images/2c1-1f-Advanced.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Advanced Settings and Sizing">  

7. **Ready to Complete**  
    * Review your configuration and click **Finish**.  
    <img src="../images/2c1-1g-Ready.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Review and Complete">  


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
    <img src="../images/2c1-4a-Supervisor.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="vCenter Server and Network Configuration">  

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
<img src="../images/2c1-5a-Validation.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Supervisor Validation Status">

#### **Validate Supervisor Content Library**  
Validate Supervisor Content Library by navigating to **vCenter** > **Supervisor Management** > **Supervisors**, select **[your supervisor]**
<img src="../images/2c1-5b-Validation.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Supervisor Validation Status">

