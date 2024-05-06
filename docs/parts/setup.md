<!-- See https://squidfunk.github.io/mkdocs-material/reference/ -->
# Part 1: Setup

## Steps

### 1. Account Setup and Terraform Install

This workshop assumes working knowledge of Equinix Metal and Terraform.  If you are not already familiar with those, we recommend that you complete the [Terraform on Equinix Metal](https://equinix-labs.github.io/terraform-on-equinix-workshop) workshop before proceeding with this workshop.

In particular, this workshop assumes that you already have an Equinix Metal account and understand how to create an Equinix Metal API token and pass it in to Terraform.  If you need a refresher on any of those steps, you may refer to the [Account Setup and Terraform Install](https://equinix-labs.github.io/terraform-on-equinix-workshop/parts/install/#part-1-account-setup-and-terraform-install) section of the Terraform workshop.

### 2. Obtain Terraform configuration

We will use the [`equinix-labs/metal-nutanix-cluster`](https://registry.terraform.io/modules/equinix-labs/metal-nutanix-cluster/equinix/latest) module to provision our Nutanix cluster.

Clone [that module's GitHub repository](https://github.com/equinix-labs/terraform-equinix-metal-nutanix-cluster) to your computer before proceeding with the workshop:

```sh
git clone https://github.com/equinix-labs/terraform-equinix-metal-nutanix-cluster
```

### 3. Verify

Make sure you have the necessary tools set up by initializing your terraform configuration:

```sh
cd terraform-equinix-metal-nutanix-cluster
terraform init
```

## Discussion

Before proceeding to the next part let's take a few minutes to discuss what we did. Here are some questions to start the discussion.

- Why does Terraform need an Equinix Metal API token?
