<!-- See https://squidfunk.github.io/mkdocs-material/reference/ -->

# Part 5: Deploy Prism Central

Many of the more powerful capabilities of Nutanix are only unlocked after configuring Prism Central. In this part of the workshop, we will deploy Prism Central on our Nutanix cluster.

## Steps

### 1. Open Prism UI

Let's return to the Prism UI from your computer at `https://localhost:9440`. Note that the Prism UI ships with a self-signed TLS certificate, so most browsers will display a security warning. This security warning is unavoidable. If your browser will allow you to ignore the warning and load the site, do that; if not, try a different browser.

### 2. Set Virtual IP and ISCSI IP for the Cluster

Prism Central requires an iSCSI IP address for the cluster. These addresses are used to provide storage to the Prism Central service.

Click on Cluster Details

<screenshot>

Set up a network
VLAN ID 0
Do not enable Nutanix to manage the IP addresses

<screenshot>

### 3. Deploy Prism Central

Click on the gear icon in the upper right corner of the Prism UI and select `Prism Central`.

<screenshot>

Choose Single Instance, Small Deployment, 255.255.252.0 / 192.168.100.2 for the Subnet and Gateway, 192.168.101.47 for the ISCSI IP, and click `Next`.

<screenshot>

### 4. Reconnect your SSH tunnel to Prism Central

After prism installs you'll need to reconnect with an updated ssh forward command that forwards to the Prism VM:

```sh
ssh -L 9440:192.168.101.47:9440 -i $(terraform output -raw ssh_private_key) root@(bastion_public_ip)
```

### 5. Log in to Prism Central

Now that the SSH tunnel is open, you can access the Prism Central UI from your computer at `https://localhost:9440`. Note that the Prism Central UI ships with a self-signed TLS certificate, so most browsers will display a security warning. This security warning is unavoidable. If your browser will allow you to ignore the warning and load the site, do that; if not, try a different browser.

### 6. Configure Prism Central

Set up DNS and NTP settings.

<screenshot>

<screenshot>

### 7. Update the Cluster

Navigate to LCM and update the cluster.

We recommend updating everything. This can take a while and will require a reboot of the cluster, but will let you get the latest version of features like the Nutanix Kubernetes Engine.

Items to Update:

- AOS
- AHV
- NCC
- Foundation
- LCM
- Prism Central
- Karbon

### 8. Wait for update to complete and continue to next Part

In the next part we'll deploy another piece of the Nutanix stack onto the cluster.

## Discussion

Before proceeding to the next part let's take a few minutes to discuss what we did. Here are some questions to start the discussion.

- What is Prism Central?
- What can you do with Prism Central that you couldn't do with just Prism?
- What is the LCM and why is it important to keep it updated?
