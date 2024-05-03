<!-- See https://squidfunk.github.io/mkdocs-material/reference/ -->

# Part 6: Deploy Nutanix Kubernetes Engine

Nutanix Kubernetes Engine (NKE) is a managed Kubernetes service that simplifies the deployment and management of Kubernetes clusters. NKE is integrated with Prism Central, so you can manage your Kubernetes clusters alongside your other Nutanix workloads.

## Steps

### 1. Open Prism UI

Let's login to the Prism Central UI from your computer at `https://localhost:9440`. Note that the Prism UI ships with a self-signed TLS certificate, so most browsers will display a security warning. This security warning is unavoidable. If your browser will allow you to ignore the warning and load the site, do that; if not, try a different browser.

### 2. Deploy Nutanix Kubernetes Engine

Click on the gear icon in the upper right corner of the Prism UI and select `Kubernetes Clusters`. Click `Create Kubernetes Cluster`. Choose `Development Cluster` as the type and name it `k8s-demo`.

<screenshot>

Leave the Node Network as `VM Network` and the number of workers as 1.

<screenshot>

We'll use the default networking provider of `calico` and the default storage class of `default`.

<screenshot>

Click `create`. You may need to refresh to see the cluster creation status.

### 3. Download the kubeconfig file

Congrats, you now have a kubernetes cluster on your Nutanix cluster.
Download the kuberconfig file via the browser:

<screenshot>

Transfer it to your bastion host:

```sh
scp kubeconfig -i $(terraform output -raw ssh_private_key) root@$(terraform output -raw bastion_public_ip):~/kubeconfig
```

<screenshot>

### 4. Access the Kubernetes cluster

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
