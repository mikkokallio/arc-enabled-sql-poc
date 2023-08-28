# arc-enabled-sql-poc
Testing the installation of SQL Server on Ubuntu, and onboarding the server to Azure Arc

* I made a fresh install of Ubuntu 20.04 on an old HP laptop.
  - Currently, SQL Server 2022 lists compatibility with Ubuntu 22.04 as a preview, while with 20.04 it is in GA.
  - I installed Ubuntu Desktop for simplicity's sake, but of course, Ubuntu Server would mirror a production scenario more closely.
* After apt installing `curl` on the machine, I started the installion of SQL Server following these instructions: https://learn.microsoft.com/en-us/sql/linux/quickstart-install-connect-ubuntu?view=sql-server-ver16&tabs=ubuntu2004
  - When I reached the part where you must choose an edition of SQL Server, I chose `9) Standard (Billed Through Azure)`. I was interested in seeing how connecting a paid version behaves with Azure Arc.
  - I also installed the optional CLI tools mentioned in the article.
* After checking that the SQL Server service was up and running on Ubuntu, I went to Azure portal and started the process of onboarding the Ubuntu server.
  - According to current documentation, the SQL Server should be onboarded automatically with the machine.
  - I set up the resource group in Sweden Central.
  - I chose the option to onboard a single server manually with a downloaded script, though later it may be interesting to also test using a service principal or other methods.
* After downloading the script, I copied it to the Ubuntu machine and ran it.
  - The script successfully connected to `aka.ms` on port 443, and after resolving the location of a bash script, it was downloaded to `/root/install_linux_azcmagent.sh`.
  - That script is automatically run, so running it manually or changing the script wasn't necessary, at least when not using a proxy. The article has an alternate command for use with a proxy.
  - The installation process of the azcmagent tries to do an IP port checking, but `netstat` was not installed on the machine, so that was skipped.
