<!-- See https://squidfunk.github.io/mkdocs-material/reference/ -->
# Part 2: Provision

Now that our local environment is set up, we are ready to use Terraform to provision our Nutanix cluster.  The module we are using provides some default behavior that we recommend following for this workshop.  It creates a new Equinix Metal project for your cluster and a new VLAN in that cluster for private networking.  It deploys 3 Nutanix nodes with private IPs and 1 bastion server that has a public IP address and can be used to access the Nutanix nodes via SSH.  The Nutanix nodes are deployed to on-demand m3.large.x86 instances.  These defaults--among other behaviors--can be changed depending on your workshop needs; we will discuss those options later in the workshop.

## Steps

### 1. Set up your Terraform variables

In order to provision your cluster with Terraform, you must set some variables that the Terraform module will use to determine how and where you cluster is deployed.  You can see examples of all of the variables supported in the module in `terraform.tfvars.example`.

<!--
TODO: duplicate the contents of that file here for reference?
-->

For this workshop, we want to use the default values of most variables so we only need to specify values for the following variables:

* `metal_auth_token` so that Terraform can create resources in Equinix Metal on your behalf
* `metal_project_name` so that it is easy to find your Nutanix cluster in the Metal console or CLI
* `metal_metro` so that Terraform knows where to deploy your infrastructure.  We recommend deploying to the Seoul metro (`sl`) for the best experience.

Create a `terraform.tfvars` file that sets the variables mentioned above.  Your `terraform.tfvars` should look something like this:

```hcl
metal_auth_token      = "<your Equinix Metal API token>"
metal_metro           = "sl"
metal_project_name    = "<your name>-nutanix-workshop"
```

### 2. Provision your infrastructure with Terraform

Now that we have configured the necessary Terraform variables, we are ready to provision our Nutanix cluster.  Terraform will prompt us to review the planned deployment before proceeding:

```sh
$ terraform apply
# ...
# ... terraform plan output (we should probably show at least some of this?)
Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: 
```

Review the terraform plan to confirm that you see the expected number of Nutanix nodes, that all infrastructure is being deployed to the expected metro, etc., and then enter `yes` to proceed with the deployment.

Each Nutanix node will take about 20 minutes to deploy.  All 3 nodes are deployed in parallel.  Once all 3 nodes are deployed successfully, there is additional cluster configuration that takes about 20 minutes as well, so you should have a working Nutanix cluster in about 40 minutes.

If any of your Nutanix nodes fails to provision, wait for Terraform to finish running and then you can re-run `terraform apply` to have Terraform attempt to replace the failed nodes; keep in mind that any provisioning failures will extend the time it takes to get a working Nutanix cluster.

### 3. Verify

## Discussion

<!--
  TODO: fill in discussion points if we need them here
-->

Before proceeding to the next part let's take a few minutes to discuss what we did. Here are some questions to start the discussion.

* How can you reduce the possibility of provisioning failures for Nutanix nodes?
* Can you change the version of Nutanix that is deployed?

