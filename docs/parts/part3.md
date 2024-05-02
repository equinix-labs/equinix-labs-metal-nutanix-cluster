<!-- See https://squidfunk.github.io/mkdocs-material/reference/ -->
# Part 3: Access the Prism UI

## Steps

### 1. Set up an SSH tunnel to the Prism UI

By default, the Nutanix Prism UI runs on port 9440 of each Nutanix CVM.  However, because we set up our Nutanix nodes on a private network, that port is not directly reachable from outside of your Equinix Metal project.  It is only reachable from the bastion server.

The Terraform module provides an output called `ssh_forward_command`.  This output contains the command needed to set up an SSH tunnel that will allow you to access the Prism UI for your Nutanix cluster from your own computer via the bastion host.

To set up the SSH tunnel, execute the value of `ssh_forward_command` as a shell command:

```sh
$(terraform output -raw ssh_forward_command)
```

If that command is successful, you will be logged in to the console of your bastion server.  Leave your console open so that the SSH tunnel stays open.

### 2. Log in to the Prism UI

Now that the SSH tunnel is open, you can access the Prism UI from your computer at `https://localhost:9440`.  Note that the Prism UI ships with a self-signed TLS certificate, so most browsers will display a security warning.  This security warning is unavoidable. If your browser will allow you to ignore the warning and load the site, do that; if not, try a different browser.

<!--
TODO: a screenshot here?
-->

Log in to the Prism UI using the default credentials (TODO: put them here? link to where they are?).  You will be forced to change the password when you first log in.  After changing the password, you will be automatically logged out.

<!--
TODO: another screenshot?
-->


### 3. Verify

Log back in with your new password to see the Prism UI.
<!--
TODO: point out some Prism UI things here?  Cluster status, licensing warnings, ???
-->

## Discussion

Before proceeding to the next part let's take a few minutes to discuss what we did. Here are some questions to start the discussion.

- Can you use the Nutanix Terraform provider with your cluster?

