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
