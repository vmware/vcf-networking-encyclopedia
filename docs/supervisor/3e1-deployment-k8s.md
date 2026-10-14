<h1>
  <img src="../../assets/VKS.png" style="height:30px; vertical-align:middle;"> Supervisor with "NSX + CTGW/Edge/T0"
</h1>

<div class="grid" markdown style="grid-template-columns: 60% 40%">

<div markdown>

This section describes the procedures for **deploying a K8s Cluster in a Namespace utilizing an "NSX + CTGW/Edge/T0" architecture** inside a vSphere environment.

* **K8s Cluster**
    * [**Deployment via vCenter UI**](#deployment_k8s)
    * [Deployment via CLI](3e2-deployment-k8s.md)
    * [Access via CLI](3e3-access-k8s.md)

</div>

<div markdown>
<img src="../images/3e1-0-k8s.jpg" style="width: 85% !important; max-width: 250px;" alt="VDS Architecture">
</div>
</div>

---

## K8s Cluster Deployment {: #deployment_k8s }

<img src="../images/3e1-1-Topology.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 500px;" alt="Topology">

### Create K8s Cluster

Navigate to **vCenter** > **Supervisor Management** > **Supervisors**, select **[your supervisor]**, navigate to **Namespaces**, select **[your namespace]**, navigate to **Resources**, and click on **Kubernetes - Create Cluster**  
<img src="../images/2e1-1-kubernetes-create.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Add Namespace Resources">

1. **Configuration Type**  
Select the **Cluster Type** and **Configuration Type**.
<img src="../images/2e1-1a-ConfType.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 800px;" alt="Select VM Deployment Source">  

1. **Review and Confirm**  
Review and click **Finish** to deploy the k8s cluster.
<img src="../images/2e1-1b-Deploy.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 800px;" alt="Select VM Deployment Source">  

??? info "More k8s cluster options"
    To get more options, such as **replicas** and **OS images**, select at step1 **Custom configuration**.



---

### Validate Deployment

#### Validate K8s Cluster Status
Once the wizard completes, verify the deployment was successful by navigating to **vCenter** > **Supervisor Management** > **Supervisors**, select **[your supervisor]**, navigate to **Namespaces**, select **[your namespace]**, navigate to **Resources**, and click on **Kubernetes - Go to Service**.  

<img src="../images/3e1-2a-Validation.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Validate K8s Cluster Status">

#### Retrieve K8s Cluster Controller VIP
Once the wizard completes, look at the K8s Cluster YAML file by navigating to the K8s Cluster Status (see above), click on **View YAML**, and find the **controlPlaneEndpoint** configuration.

<img src="../images/3e1-2b-K8sVIP.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 600px;" alt="Find K8s Cluster VIP">

#### Check K8s Cluster VMs in vCenter Inventory
Verify the K8s Cluster VMs are running by navigating to **vCenter** > **Inventory**, select the **K8s VM in the Namespace**.
<img src="../images/3e1-2c-Validation.jpg" style="display: block; margin: 0 auto; width: 85% !important; max-width: 900px;" alt="Validate K8s Cluster VMs">

