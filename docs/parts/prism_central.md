<!-- See https://squidfunk.github.io/mkdocs-material/reference/ -->

# Part 5: Deploy Prism Central

Many of the more powerful capabilities of Nutanix are only unlocked after configuring Prism Central. In this part of the workshop, we will deploy Prism Central on our Nutanix cluster.

## Steps

### 1. Open Prism UI

Let's return to the Prism UI from your computer at `https://localhost:9440`. Note that the Prism UI ships with a self-signed TLS certificate, so most browsers will display a security warning. This security warning is unavoidable. If your browser will allow you to ignore the warning and load the site, do that; if not, try a different browser.

### 2. Set up Virtual IP, ISCSI IP, and NTP for the Cluster

Prism Central requires an iSCSI IP address for the cluster. This address is used to provide storage to the Prism Central service.

In the next part we'll be installing a service that requires the cluster have a virtual IP as well as NTP configured, so we'll be configuring that now as well.

Click on the gear icon in the upper right corner of the Prism UI. Then choose `Cluster Details` and enter `192.168.103.254` for the Virtual IP and `192.168.103.253` for the ISCSI Data Services IP and and click `Save`.

![Cluster Details](../images/iscsi-ip.png)

We also need to configure NTP for the cluster. Click on the gear icon in the upper right corner of the Prism UI and select `NTP Servers` from the settings menu on the left. Then add a local NTP pool server for the region you deployed Nutanix into. We'll use `0.north-america.pool.ntp.org`

![NTP Servers](../images/ntp-servers.png)

### 3. Deploy Prism Central

Next, choose `Prism Central Registration` and select `Register or create new`.

![Prism Central Registration](../images/prism-central-deploy.png)

Choose `Deploy`, pick whatever version it offers you and click `Next`.

![prism-central-version](../images/prism-central-version.png)

and then `Deploy Single-VM PC` and click `Next`.

![Single VM](../images/single-vm.png)

Then choose `Small` and choose `Create Network` from the `Network` drop down.

![Small Deployment](../images/small-deployment.png)

Enter `VM Network` and VLAN ID `0` and click `Save`.

![VM Network](../images/vm-network.png)

Now enter `255.255.252.0` for the Subnet Mask and `192.168.100.2` for the Gateway, and `192.168.103.252` for the VM IP and click `Next`.

![VM More Network](../images/vm-more-network.png)

Now be patient while Prism Central installs. This can take a while. When finished, you'll be back at the `Register or create new` screen again. That's normal. Let's verify Prism Central is installed by checking the VM tab and looking for the Prism Central VM. This screenshot has highlighted ways to identify the VM.

![Prism Central VM Tab](../images/prism-central-vm-tab.png)

### 4. Register the Prism Central VM

Now that Prism Central is deployed, we need to register the cluster to it. Click on the gear icon in the upper right corner of the Prism UI and select `Prism Central Registration`. Click `Register or create new` and then click `Connect` under `I already have a Prism Central instance deployed`.

![Prism Central Registration](../images/prism-central-registration.png)

Click `Next` on the `Connect Info` page and then put in the Prism Central IP address `192.168.103.252`, port of `9440`, and the username and password you used to log in to Prism. This will be the default Prism password. Click `Next`.

![Prism Central Registration Config](../images/prism-central-registration-config.png)

After a few seconds, registration will complete. You will need to refresh the page to see the updated registration status. Now we can go login to Prism Central.

![Prism Central Connected](../images/prism-central-connected.png)

### 5. Reconnect your SSH tunnel to Prism Central

After verifying that Prism Central is installed, you'll need to reconnect with an updated ssh forward command that forwards to the Prism VM. First exit the current SSH tunnel and then run this one:

```sh
ssh -L 9440:192.168.103.252:9440 -i $(terraform output -raw ssh_private_key) root@$(terraform output -raw bastion_public_ip)
```

### 6. Log in to Prism Central

Now that the SSH tunnel is open, you can access the Prism Central UI from your computer at `https://localhost:9440`. Note that the Prism Central UI ships with a self-signed TLS certificate, so most browsers will display a security warning. This security warning is unavoidable. If your browser will allow you to ignore the warning and load the site, do that; if not, try a different browser.

Log in to the Prism UI using the default credentials [found here on the Nutanix Support Portal](https://portal.nutanix.com/page/documents/details?targetId=AHV-Admin-Guide-v6_7:app-admin-cvm-access-c.html). You will be forced to change the password when you first log in. After changing the password, you will be automatically logged out.

### 7. Configure Prism Central

Let's start setting up some basic settings in Prism Central. Start by clicking the gear icon in the upper right corner of the Prism Central UI and selecting `Name Servers` from the settings menu on the left. Then add some public DNS servers to the list, we'll use `1.1.1.1` and `8.8.8.8`.

![Name Servers](../images/name-servers.png)

Next click on NTP Servers on the left and add a local NTP pool server for the region you deployed Nutanix into. We'll use `0.north-america.pool.ntp.org`

![Prism Central NTP Servers](../images/prism-central-ntp-servers.png)

In the next part we'll deploy another piece of the Nutanix stack onto the cluster.

### 9. Wait for update to complete and continue to next Part

In the next part we'll deploy another piece of the Nutanix stack onto the cluster.

## Discussion

Before proceeding to the next part let's take a few minutes to discuss what we did. Here are some questions to start the discussion.

- What is Prism Central?
- What can you do with Prism Central that you couldn't do with just Prism?
- What is the LCM and why is it important to keep it updated?
- Why do we need to use a new SSH forward command after installing Prism Central?
