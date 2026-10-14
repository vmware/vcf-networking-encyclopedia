<h1>
  <img src="../../assets/VKS.png" style="height:30px; vertical-align:middle;"> Supervisor with "VDS + FLB"
</h1>

<div class="grid" markdown style="grid-template-columns: 60% 40%">

<div markdown>

This section describes the procedures for **deploying the Supervisor Namespace utilizing a "VDS + FLB" architecture** inside a vSphere environment.

* **Namespace**
    * [**Deployment**](#namespacedeployment)
    * [Access via CLI](1c2-access-namespace.md)

</div>

<div markdown>
<img src="../images/1c1-0-Namespace.jpg" style="width: 85% !important; max-width: 250px;" alt="VDS Architecture">
</div>
</div>

---

## Namespace Deployment {: #namespacedeployment }

<img src="../images/1c1-0-Topology.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Topology">


### Create Namespace
Navigate to **vCenter** > **Supervisor Management** > **Namespaces**, and click **NEW NAMESPACE**.
<img src="../images/1c1-1-AddSupervisor.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Add Namespace">

1. **Location**  
   Select the **Supervisor**, and click **Next**.  
    <img src="../images/1c1-1a-SupervisorNamespace.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 800px;" alt="Select Supervisor Location">  

2. **Configuration**  
   Give a name to the **Namespace**, select at least 1 **Network** to be used by the Namespace, and click **Next**.  
    <img src="../images/1c1-1b-SupervisorNamespace.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 800px;" alt="Name the Namespace">  

3. **Add Zones**  
   Select the **Workload Zone** (one or more vCenter Clusters), and click **Next**.  
    <img src="../images/1c1-1c-SupervisorNamespace.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 800px;" alt="Add Workload Zones">  

4. **Review**  
   Review the Namespace settings, and click **Finish**.  
    <img src="../images/1c1-1d-SupervisorNamespace.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 800px;" alt="Review Namespace Settings">  


---

### Finish Namespace Creation for future Applications

#### **Create a Content Library for future VMs**  
If you plan to deploy VMs, create a Content Library with VM images.  
Navigate to **vCenter** > **Content Libraries** > and click **Create**.  
<img src="../images/2c1-2a-ContentLibrary.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Create Content Library">  

1. **Name and Location** Give it a **Name** and select the **vCenter** hosting that Content Library, and click **Next**.  
    <img src="../images/2d1-2b-Name.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 800px;" alt="Name Content Library">  
    
1. **Configure Content Library** Choose between **Local content library** (you upload VM images) and **Subscribed content library** (vCenter downloads VM images from a repository), and click **Next**.  
    *I'm using here Local content library.* <img src="../images/2d1-2c-ContentLibrary.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 800px;" alt="Configure Content Library Type">  

1. **Apply Security Policy** Apply **security policy** if you choose to, and click **Next**.  
    *I'm using here no security policy.* <img src="../images/2d1-2d-SecurityPolicy.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 800px;" alt="Apply Security Policy">  

1. **Add Storage** Select a **storage** to host the content library images, and click **Next**.  
    <img src="../images/2d1-2e-Storage.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 800px;" alt="Select Storage">  

1. **Ready to complete** Review the content library, and click **Finish**.  
    <img src="../images/2d1-2f-Review.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 800px;" alt="Review Content Library Configuration">  

1. **Import VM Image** In the Content Library created, import a VM Image, click on **Import Item**.  
    <img src="../images/2d1-2g-ImportItem.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 800px;" alt="Import VM Image Item">  

1. **Choose VM Image** Choose a **Source file** via URL or Local file, and click **Import**.  
    *I'm using here the URL https://cloud-images.ubuntu.com/noble/current/noble-server-cloudimg-amd64.ova to download Ubuntu 24.04.* <img src="../images/2d1-2h-ImportURL.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 600px;" alt="Import VM Image URL">  

#### **Associate the Content Library to the Namespace**  
Navigate to **vCenter** > **Supervisor Management** > **Supervisors**, select **[your supervisor]**, navigate to **Namespaces**, and select **[your namespace]**, and click on **VM Service - Add Content Library**.
<img src="../images/2d1-3a-NameSpace.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Navigate to Namespace Content Library">  

1. **Add Content Library** Select **Content Library with VM images**, and click **OK**.  
    <img src="../images/2d1-3b-AddContentLibrary.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 600px;" alt="Select Content Library">  

#### **Associate Storage Policy to the Namespace**
Navigate to **vCenter** > **Supervisor Management** > **Supervisors**, select **[your supervisor]**, navigate to **Namespaces**, and select **[your namespace]**, and click on **Storage - Add Storage**.
<img src="../images/2d1-4a-NameSpace.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Navigate to Namespace Storage">  

1. **Add Storage Policy** Select **Storage Policy**, and click **OK**.  
    <img src="../images/2d1-4b-AddStorage.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 600px;" alt="Select Storage Policy">  


#### **Associate VM Class to the Namespace**  
Navigate to **vCenter** > **Supervisor Management** > **Supervisors**, select **[your supervisor]**, navigate to **Namespaces**, and select **[your namespace]**, and click on **VM Service - Add VM Class**.
<img src="../images/2d1-3a-NameSpace.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Navigate to Namespace VM Class">  

1. **Add VM Classes** Select **VM Classes**, and click **OK**.  
*I selected here all VM Classes type "small" and "medium".*
<img src="../images/2d1-5b-AddVMClass.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 800px;" alt="Add VM Classes">  



---

### Validate Deployment

#### **Validate Namespace Status** 
Once the wizard completes, verify the deployment was successful by navigating to **vCenter** > **Supervisor Management** > **Supervisors**, select **[your supervisor]**, and navigate to **Namespaces**.

<img src="../images/1c1-6a-Validation.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Validate Namespace Status">

#### **Validate Namespace Storage and Content Library** 
Verify the Namespace has at least a **Content Library**, **VM Classes**, and **Storage** by navigating to **vCenter** > **Supervisor Management** > **Supervisors**, select **[your supervisor]**, navigate to **Namespaces**, and select **[your namespace]**.
<img src="../images/2d1-6b-Validation.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Validate Namespace Resources">

