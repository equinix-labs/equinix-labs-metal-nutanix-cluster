<!-- See https://squidfunk.github.io/mkdocs-material/reference/ -->

# Part 6: Deploy Nutanix Kubernetes Engine

Nutanix Kubernetes Engine (NKE) is a managed Kubernetes service that simplifies the deployment and management of Kubernetes clusters. NKE is integrated with Prism Central, so you can manage your Kubernetes clusters alongside your other Nutanix workloads.

## Steps

### 1. Open Prism UI

Let's login to the Prism Central UI from your computer at `https://localhost:9440`. Note that the Prism UI ships with a self-signed TLS certificate, so most browsers will display a security warning. This security warning is unavoidable. If your browser will allow you to ignore the warning and load the site, do that; if not, try a different browser.

### 2. Update the Cluster

Navigate to LCM take an Invenotry, wait for it to finish and then Navigate to Software -> Updates.

We recommend updating the Nutanix Kubernetes Engine.

### 3. Deploy Nutanix Kubernetes Engine

Click on the three lined menu in the upper left of the Prism Central UI and select `Services` -> `Kubernetes`. Then click `Enable Kubernetes`. After awhile Karbon will start up and you can click `Download OS Image`. Wait until the `Download Status` is `Downloaded`.

![Karbon Download Status](../images/nke-download-status.png)

Now back to Clusters on the left and click `Create Kubernetes Cluster`.

![Karbon Create Cluster](../images/nke-create-cluster.png)

Choose `Development Cluster` as the type and click `Next`.

![Karbon Development Cluster](../images/nke-development-cluster.png)

Name it `k8s-demo` and click `Next`.

![K8S Demo Name](../images/k8s-demo.png)

Leave the Node Network as `VM Network` and the number of workers as 1 and click `Next`.

![K8S Demo Resources](../images/k8s-demo-resources.png)

We'll use the default networking provider, so just click `Next`.

![K8s Demo Network Provider](../images/k8s-demo-network-provider.png)

On the Storage Class page, fill in the cluster username and password, leave the rest of the options as their defaults, and click `Create`.

![K8s Demo Storage Class](../images/k8s-demo-storage-class.png)

Click `Create`. You may need to refresh to see the cluster creation status. Now just wait for the cluster to finish deploying.

### 4. Download the kubeconfig file

Congrats, you now have a kubernetes cluster on your Nutanix cluster.
Download the kuberconfig file via the browser:

![Download Kubeconfig](../images/download-kubeconfig.png)

Transfer it to your bastion host:

```sh
scp k8s-demo-kubectl.cfg -i $(terraform output -raw ssh_private_key) root@$(terraform output -raw bastion_public_ip):~/kubeconfig
```

### 5. Access the Kubernetes cluster

Now that you have the kubeconfig file on your bastion host, you can access the Kubernetes cluster from your computer. First, SSH into the bastion host:

```sh
ssh -i $(terraform output -raw ssh_private_key) root@$(terraform output -raw bastion_public_ip)
```

Then, set the `KUBECONFIG` environment variable to point to the kubeconfig file:

```sh
export KUBECONFIG=$(PWD)/kubeconfig
```

Now you can use `kubectl` to interact with your Kubernetes cluster:

```sh
kubectl get nodes
```

```sh
kubectl get pods -A
```

## Discussion

Before proceeding to the next part let's take a few minutes to discuss what we did. Here are some questions to start the discussion.

- What is Nutanix Kubernetes Engine?
- How does NKE integrate with Prism Central?
- What are some use cases for NKE?
- How does NKE compare to other managed Kubernetes services?
