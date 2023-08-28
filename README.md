# arc-enabled-sql-poc
Testing the installation of SQL Server on Ubuntu, and onboarding the server to Azure Arc

* I made a fresh install of Ubuntu 20.04 on an old HP laptop.
  - Currently, SQL Server 2022 lists compatibility with Ubuntu 22.04 as a preview, while with 20.04 it is in GA.
  - I installed Ubuntu Desktop for simplicity's sake, but of course, Ubuntu Server would mirror a production scenario more closely.
* After apt installing `curl` on the machine, I started the installion of SQL Server following these instructions: https://learn.microsoft.com/en-us/sql/linux/quickstart-install-connect-ubuntu?view=sql-server-ver16&tabs=ubuntu2004
  - When I reached the part where you must choose an edition of SQL Server, I chose `9) Standard (Billed Through Azure)`. I was interested in seeing how connecting a paid version behaves with Azure Arc.
  - I also installed the optional CLI tools mentioned in the article.
* After checking that the SQL Server service was up and running on Ubuntu, I went to Azure portal and started the process of onboarding the Ubuntu server following these instructions: https://learn.microsoft.com/en-us/azure/network-watcher/connection-monitor-connected-machine-agent?tabs=LinuxScript
  - According to current documentation, the SQL Server should be onboarded automatically with the machine.
  - I set up the resource group in Sweden Central.
  - I chose the option to onboard a single server manually with a downloaded script, though later it may be interesting to also test using a service principal or other methods.
* After downloading the script, I copied it to the Ubuntu machine and ran it. The script does a bunch of things if you run every command in it. These include (but are not limited to):
  - The script successfully connected to `aka.ms` on port 443, and after resolving the location of a bash script, it was downloaded to `/root/install_linux_azcmagent.sh`. This new script is also run, which actually installs the agent.
  - The article I was following asked to manually run `install_linux_azcmagent.sh` but as mentioned above, the original installation script includes a line for running it, so there's no need to do that again. Furthermore, in the article, there is a typo: the first letter in the name should not be uppercase.
  - If you're using a proxy server, then you actually need to run a slightly different version of the command, as mentioned in the article.
  - The installation process of the azcmagent tries to do an IP port check, but `netstat` was not installed on the machine, so that was skipped. On a server behind a firewall, that check might be useful, or running a similar check manually.
  - After running the last command which actually connects the agent to Azure, you have open a browser session at `https://microsoft.com/devicelogin` and enter a code to authenticate. I performed the authentication on a different laptop.
  - After authenticating, the agent proceeded to creating the connection / resource in Azure.
* In Azure, the Ubuntu laptop now showed up as a resource. The SQL Server did not despite waiting more than an hour. Obviously the automatic deployment didn't get triggered for some reason.
* I then tried logging into the SQL Server with `sqlcmd -S localhost -U sa` but after providing the password, I got an error about certificate verify having failed. This was fixed by adding `-C` to the command. The article also mentions `-No` as a way around it.
  - Just as a test, I created a db, table, and a few rows of data. Everything seemed to work fine.
* Back in Azure, I wiewed the Arc-enabled laptop's extensions and from there, started installing the SQL extension.
  - It gives three options, "PAYG", "Paid", and "LicenseOnly". I chose "PAYG" in accordance with my earlier choice.
  - That selection appears to also change the selection on the SQL Server Configuration blade of the server. The page shows the same three options.
  - The deployment took a really long time... or rather didn't finish.
  - Region may be a problem: I discovered in docs that Arc-enabled SQL Server is not available in Sweden Central.
* Realizing the region may have been the problem, I proceeded to remove the agent using this: https://learn.microsoft.com/en-us/azure/azure-arc/servers/manage-agent?tabs=windows
* Then I gave it another try, putting everything in West Europe.
  - This time I also granted right for Automanage to do stuff.
  - First attempt to connect agent to Azure failed with 403. Failed to Get MSI Certificate from HIS.
  - Second attempt was really quick and succeeded. The resource appeared in Azure after a few minutes.
- SQL Server didn't show up at the same time, but in Extensions, I can see it's being created. SQL Server Configuration doesn't yet show any of the three options chosen. `ps aux` on the Ubuntu machine shows the extension running as a process.
