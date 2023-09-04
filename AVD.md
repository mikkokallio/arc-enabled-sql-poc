# AVD PoC

I started a PoC in my own environment.

1. First try to set up an AVD environment, I tried the Getting started wizard in Azure portal.
* I had no AD DS, so the wizard provisioned AAD DS for me.
* I chose North Europe as the region.
* I put the same email in Azure admin and domain admin.
* I created two VMs with the Win 11 Enterprise multi-session + Microsoft 365 Apps, newest version.
* I also created a test user with a different email address that I own.
* Validation passed, so I hit Create.

This resulted in an error in the deployment of `easy-button-inputvalidation-job-linked-template`.

"{"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/arm-deployment-operations for usage details.","details":[{"code":"Conflict","message":"{\r\n  \"status\": \"Failed\",\r\n  \"error\": {\r\n    \"code\": \"ResourceDeploymentFailure\",\r\n    \"message\": \"The resource write operation failed to complete successfully, because it reached terminal provisioning state 'Failed'.\"\r\n  }\r\n}"}]}"

2. This time trying to create AAD DS first so that it's clear what domain name you'll have. Otherwise, it's the same subscription, North Europe, etc. Failed because Standard Edition doesn't support cloud-only sync.

3. Trying again with sync type as All, keeping Standard SKU. Provisioning takes a long time but so far looks like it worked this time.

4. Provisioned a vnet for host pool. Peered it with the AAD DS vnet.

5. 
