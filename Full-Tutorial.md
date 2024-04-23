Honey Net Lab Steps with Screenshots.

# Contents
# Table of Contents
- [Creating First Resource](#creating-first-resource)
- [Installing MS SQL Server](#installing-ms-sql-server)
- [Precursor to Security Operations (Failed Auth, Log Observation)](#precursor-to-security-operations-failed-auth-log-observation)
- [Azure Active Directory (now renamed to Microsoft Entra ID)](https://github.com/Checkerss/Azure-Honey-Net-Lab/blob/master/tut.md#azure-active-directory-now-renamed-to-microsoft-entra-id-)
- [Logging and Monitoring](#logging-and-monitoring)
- [Enabling Microsoft Defender](#enabling-microsoft-defender)
- [Enable Log Collection for VMs and Network Security Groups](#enable-log-collection-for-vms-and-network-security-groups)
- [Kusto Query Language (KQL)](#kusto-query-language-kql)
- [Tenant Level Logging](#tenant-level-logging)
- [Next Will be doing Subscription Level Logging (Activity Log)](#next-will-be-doing-subscription-level-logging-activity-log)

# *<u>CREATING FIRST RESOURCE</u>*

**Step 1:** Create your Azure account using one of three options: free,
pay-as-you-go, or student. Choose whichever fits your needs. Once you
have done so, you will be redirected to the Azure home portal.

<img src="./media/media/image2.png" style="width:6.5in;height:5.97778in"
alt="A screenshot of a computer Description automatically generated" />

**Step 2: <u>Create Windows 10 Pro Virtual Machine (Name it
windows-vm)</u>**

- See all sizes, cheap-ish, strong password

- Region: EAST US 2

- Name the Resource Group: **<u>RG-Cyber-Lab</u>**

- Name the Virtual Network. **<u>NAME IT “Lab-VNet”</u>**

- **<u>Username: labuser</u>**

- **<u>Password: Cyberlab123!</u>**

  - Look over screenshots to make sure your lab is the same

> <img src="./media/media/image3.png"
> style="width:5.1722in;height:3.64044in"
> alt="A screenshot of a computer Description automatically generated" />
> <img src="./media/media/image4.png"
> style="width:4.91702in;height:5.26951in"
> alt="A screenshot of a computer Description automatically generated" />
>
> <img src="./media/media/image5.png" style="width:6.5in;height:6.32778in"
> alt="A screenshot of a computer Description automatically generated" />
>
> STEP 3: **Create one more Virtual Machine running <u>Ubuntu
> (Linux)</u>. Name <u>it: “Linux-vm”</u>**

- Same Region, Resource Group, and VNet as windows-vm

- Region: EAST US 2

- **Do not choose B1s for the VM size; choose something more
  significant, or it’ll get DDOS’d and stop creating logs, lol. I
  patched the video to convey this.**

- Ensure you use a username and password instead for authentication

**Open up Network Security Groups for both VMs:**

- Configure Network Security Group (Layer 4 Firewall) to allow
  **<u>all</u>** traffic inbound

> <img src="./media/media/image6.png"
> style="width:3.9109in;height:3.34265in"
> alt="A screenshot of a computer Description automatically generated" />
> <img src="./media/media/image7.png"
> style="width:3.9912in;height:3.77714in"
> alt="A screenshot of a computer Description automatically generated" />

- Find the NSG(network security groups) in the search bar, on the VM
  itself, or in the RG-Cyber_Lab group

> <img src="./media/media/image8.png" style="width:6.5in;height:2.99861in"
> alt="A screenshot of a computer Description automatically generated" />
>
> <img src="./media/media/image9.png"
> style="width:6.58072in;height:3.35417in"
> alt="Delete the inbound rule for 22,click on add+ to add a new inbound rule allowing all traffic. The priority has to be below the other rules, so 110 or 100 work." />

<img src="./media/media/image10.png"
style="width:5.35779in;height:2.25302in"
alt="A screenshot of a computer Description automatically generated" />

Do the same for the Windows Network Security Group: delete the RDP port
and add a new rule to allow all traffic.

<u>Keep track of Cost management.</u>

# *<u>INSTALLING MS SQL SERVER</u>*

STEP 4: **<u>Disable Windows Firewall, and Install SQL Server, and
Create Vulnerabilities</u>**

- Remote Into the VM (windows-vm)

  - You can do this with a remote desktop if you are using a Windows PC
    or download an app for Mac OS or Windows. (Google is your friend)

  - <img src="./media/media/image11.png"
    style="width:2.43441in;height:2.92731in"
    alt="A screenshot of a computer Description automatically generated" />

- Turn off the Windows Firewall

  - <img src="./media/media/image12.png"
    style="width:6.34703in;height:3.58184in"
    alt="A screenshot of a computer Description automatically generated" />

- Install SQL Server Evaluation:
  [<u>https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2019</u>](https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2019)
  (inside windows VM)

- Put fake info. It works. Click on Download Media and put it on the
  desktop.

- <img src="./media/media/image13.png"
  style="width:6.5in;height:3.59444in"
  alt="A screenshot of a computer Description automatically generated" />

- <img src="./media/media/image14.png"
  style="width:6.5in;height:5.00139in"
  alt="A screenshot of a computer Description automatically generated" />

> <img src="./media/media/image15.png"
> style="width:6.5in;height:2.98611in"
> alt="A screenshot of a computer Description automatically generated" />

- Right-click it and press mount

- Then you'll see in the image below it's now mounted, and click on
  setup to install the sql server onto the Windows VM.

> <img src="./media/media/image16.png"
> style="width:6.5in;height:3.54931in"
> alt="A screenshot of a computer Description automatically generated" />

- Click on installation and the first option,

- Click next on everything to accept the license agreement

> <img src="./media/media/image17.png"
> style="width:6.5in;height:4.87292in"
> alt="A screenshot of a computer Description automatically generated" />

- Click on database engine services and the next

> <img src="./media/media/image18.png" style="width:6.5in;height:3.9in"
> alt="A screenshot of a computer Description automatically generated" />

- Next, until you get here, click on mixed mode and input a password. I
  used the same as the VMs. Add current User, also

- Continue and install. Don’t change anything else.

> <img src="./media/media/image19.png"
> style="width:5.69968in;height:4.29546in"
> alt="A screenshot of a computer Description automatically generated" />
>
> <img src="./media/media/image20.png"
> style="width:6.5in;height:4.70833in"
> alt="A screenshot of a computer Description automatically generated" />

- Username: sa (default)

- Password: Cyberlab123! 

<!-- -->

- Install SSMS (SQL Server Management Studio):
  [<u>https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms</u>](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms) 

> <img src="./media/media/image21.png"
> style="width:3.46833in;height:3.03547in"
> alt="A screenshot of a computer Description automatically generated" />

- Enable logging for SQL Server to be ported into Windows Event Viewer
  ([<u>Ref</u>](https://learn.microsoft.com/en-us/sql/relational-databases/security/auditing/write-sql-server-audit-events-to-the-security-log?view=sql-server-ver16))

- Open the Windows registry by searching on the Windows search box
  registry editor

> <img src="./media/media/image22.png"
> style="width:6.5in;height:3.16458in"
> alt="A screenshot of a computer Description automatically generated" />
>
> <img src="./media/media/image23.png"
> style="width:5.70883in;height:4.09202in"
> alt="A screenshot of a computer Description automatically generated" />

- Go to the specified path on the ref site. Right-click security and go
  to permissions. Then ADD NETWORK SERVICE and give it full control.

> <img src="./media/media/image24.png"
> style="width:6.00885in;height:6.60057in"
> alt="A screenshot of a computer Description automatically generated" />

- Apply changes and click ok

> <img src="./media/media/image25.png"
> style="width:4.75875in;height:4.65874in"
> alt="A screenshot of a computer screen Description automatically generated" />

- Scroll to the configure portion, open command line in the VM, and
  follow the steps

<img src="./media/media/image26.png"
style="width:6.33529in;height:3.91015in"
alt="A screenshot of a computer Description automatically generated" /><img src="./media/media/image27.png"
style="width:6.35119in;height:3.23125in"
alt="A screenshot of a computer Description automatically generated" />

- Exit out everything now and, open SQL server management, and enter
  creds from before

<img src="./media/media/image28.png"
style="width:6.5in;height:3.65486in"
alt="A computer screen shot of a computer screen Description automatically generated" />

<img src="./media/media/image29.png"
style="width:3.867in;height:3.76699in"
alt="A screenshot of a computer Description automatically generated" />

- Make sure to trust the certificate box to trust the server login. Then
  Right-click on windows-vm and, click on properties, the security, and
  click on 3<sup>rd</sup> option under login auditing. And ok, after
  you're done.

- Right-click on the same windows-vm and restart the server

<img src="./media/media/image30.png"
style="width:6.5in;height:3.59931in"
alt="A screenshot of a computer Description automatically generated" />

- Disconnect and reconnect with plug icons after restart and input a
  wrong password then cancel and exit once error is shown and open event
  viewer to make sure its being audited.

<img src="./media/media/image31.png"
style="width:2.3502in;height:0.76673in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image32.png"
style="width:6.5in;height:3.55903in"
alt="A screenshot of a computer Description automatically generated" />

It looks to be working fine.

- Test SQL logging to make sure it’s working properly

**<u>Test ping and logging into Linux-vm via SSH</u>**

- Ping Linux-vm

- Login to Linux-vm

**We will use both VMs in the next video, but you can turn them off for
now if you want to save money.**

# *<u>PRECURSOR TO SECURITY OPERATIONS (FAILED AUTH, LOG OBSERVATION)</u>*

<img src="./media/media/image33.png"
style="width:6.5in;height:3.52014in"
alt="A diagram of a group Description automatically generated" />

**Check your Subscription’s [<u>Cost
Analysis</u>](https://portal.azure.com/#view/Microsoft_Azure_CostManagement/Menu/~/costanalysis/openedBy/AzurePortal)!!!!**

**<u>Admin Mode (pretend you are normal admin):</u>**

Create another Windows VM **<u>in a region outside the US and NAME IT
“attack-vm”</u>**

- Name the Resource Group RG-Cyber-Lab-Attacker

- Name the VNet Lab-VNet-Attacker

- It's the same as other VMs for space, or one CPU works also since
  Azure might not allow you to have more than two v2cpu VMs.

- Log into the VM to make sure it works

- Retrieve the public IP address of “windows-vm” from the Azure Portal
  save it for the next steps

**<u>Attacker Mode (pretend you are an attacker):</u>**

**<u>Generated some failed RDP logs against “windows-vm.”</u>**

From within of “attack-vm,” attempt to RDP into “windows-vm” **<u>with
the wrong credentials.</u>**

Repeat this step 2 more times with the wrong username and password.

<img src="./media/media/image34.png"
style="width:4.44205in;height:4.6754in"
alt="A screenshot of a computer Description automatically generated" />

**<u>Generated some failed MS SQL Auth logs against “windows-vm.”</u>**

Still within “attack-vm,” install SSMS if not already installed

Attempt to connect to the SQL Server on “windows-vm” with a wrong
password

**<u>Generated some failed SSH logs against “Linux-vm”</u>**

Still within “attack-vm,” attempt to SSH into “Linux-vm” with the wrong
credentials.

Log out of “attack-vm” now you are back to your computer.

<img src="./media/media/image35.png"
style="width:6.5in;height:5.33611in" />

You can shut down attack-vm for now.

**<u>Admin Mode (pretend you are normal admin):</u>**

From your computer, RDP back into “windows-vm.”

Inspect the failures and successes (Security Log for RDP, Application
Log for SQL)

Take note of the EventIDs, messaging, Source IP Addresses, etc.

<img src="./media/media/image36.png"
style="width:6.5in;height:6.17014in"
alt="A computer screen shot of a computer Description automatically generated" />

You can also filter event ID 4625 and see who else has been trying to
get into your VM.

Next Image shows the application logs witch is the SQL server.

<img src="./media/media/image37.png"
style="width:6.5in;height:5.12153in"
alt="A screenshot of a computer Description automatically generated" />

SSH into the Linux VM and observe the logs with the following commands:

cat /var/log/auth.log \| grep password

cat /var/log/auth.log \| grep Accepted

**TURN VMs OFF! (do not need for next STEPS)**

# *<u>Azure Active Directory (now renamed to Microsoft Entra ID )</u>*

# *<u>Overview – Users, Groups, and Access Management</u>*

**Check your Subscription’s [<u>Cost
Analysis</u>](https://portal.azure.com/#view/Microsoft_Azure_CostManagement/Menu/~/costanalysis/openedBy/AzurePortal)**

**<u>Configure and Observe Tenant-Level Global Reader</u>**

Create a user within Azure Active Directory (AAD) **(username:
globalreaderjohn)**

<img src="./media/media/image38.png"
style="width:6.1172in;height:4.85875in"
alt="A screenshot of a computer Description automatically generated" />

- Assign Tenant-Level Global Reader<img src="./media/media/image39.png"
  style="width:6.18766in;height:3.77077in"
  alt="A screenshot of a computer Description automatically generated" />

- In a new browser/incognito, log in as **globalreaderjohn** and observe
  the result of being a Tenant Level “Global Reader.”

- Close browser/incognito when satisfied

**<u>Configure and Observer Subscription Reader</u>**

Back in the main browser, create another user within AAD ** (username:
subreaderjane)**

- Assign Subscription-Level Reader By going into the search bar in the
  portal and searching subscriptions. Yours will have a different name.

> <img src="./media/media/image40.png"
> style="width:5.12544in;height:4.42538in"
> alt="A screenshot of a computer Description automatically generated" />
>
> <img src="./media/media/image41.png"
> style="width:4.53373in;height:7.47565in"
> alt="A screenshot of a computer Description automatically generated" />
>
> <img src="./media/media/image42.png"
> style="width:6.5in;height:7.07569in"
> alt="A screenshot of a computer Description automatically generated" />

- In a new browser/incognito, log in as **subreaderjane** and observe
  the result of being a Subscription Level “Global Reader.”

- Close browser/incognito when satisfied

**<u>Configure and Observe Resource Group Contributor (like an
admin)</u>**

Back in the main browser, create another user within AAD ** (username:
rgcontributordave)**

- Create a new resource group called “Permissions-Tester.”

> <img src="./media/media/image43.png"
> style="width:6.29221in;height:3.49197in"
> alt="A screenshot of a computer Description automatically generated" />

- Assign Resource Group-level Contributor

  - Click on permission group, then access control, and add role
    assignment.

> <img src="./media/media/image44.png"
> style="width:5.30879in;height:4.83375in"
> alt="A screenshot of a computer Description automatically generated" />
>
> <img src="./media/media/image45.png"
> style="width:6.5in;height:3.33194in"
> alt="A screenshot of a computer Description automatically generated" />

- For our resource group (Permission-Tester), assign Contributor
  Permissions

- In a new browser/incognito, log in as **rgcontributordave** and
  Observe the result of being a Resource Group Level Contributor.

- You can only add resources with contributors to assigned resource
  groups.

# *<u>Logging and Monitoring</u>*

<img src="./media/media/image46.png"
style="width:6.82872in;height:3.91104in"
alt="A screenshot of a computer Description automatically generated" />

**<u>Do you need your VMs to be on for this lab?</u>**

**NO**

**<u>Put Large Geo-Data Files in Azure Storage</u>**

Download this file onto your PC:

- [<u>https://github.com/joshmadakor1/Cyber-Course-v2/blob/main/Sentinel-Maps(JSON)/geoip-summarized.csv</u>](https://github.com/joshmadakor1/Cyber-Course-v2/blob/main/Sentinel-Maps(JSON)/geoip-summarized.csv) 

> <img src="./media/media/image47.png"
> style="width:6.5in;height:3.74236in"
> alt="A screenshot of a computer Description automatically generated" />

Create a Log Analytics Workspace (our log aggregator) named:
LAW-Cyber-Lab-0x

<img src="./media/media/image48.png"
style="width:6.34222in;height:7.50898in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image49.png"
style="width:6.20887in;height:7.49232in"
alt="A screenshot of a computer Description automatically generated" />

Setup Sentinel and connect it to our Log Analytics Workspace

<img src="./media/media/image50.png"
style="width:5.06711in;height:3.80866in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image51.png"
style="width:6.5in;height:5.31667in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image52.png"
style="width:4.00035in;height:7.3423in"
alt="A screenshot of a computer Description automatically generated" />

**<u>DO NOT MESS THIS UP; FOLLOW NAMES AND EVERYTHING EXACTLY</u>**

<u>Create the GeoIP watchlist:</u>

Name/Alias: GeoIP

Source type: Local File

Number of lines before row: 0

Search Key: network

<img src="./media/media/image53.png"
style="width:5.48381in;height:7.51732in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image54.png"
style="width:6.5in;height:5.48403in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image55.png"
style="width:5.56715in;height:7.55899in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image56.png"
style="width:6.5in;height:3.09722in"
alt="A screenshot of a computer Description automatically generated" />

Allow time for these files to “upload”/load from your storage account
into Sentinel/Log Analytics Workspace. There are about **27k**
rows/records

Next, go to Log Analytics Workspace and make sure something comes out
when you query \_GetWatchlist("GeoIP")

It should look like this:

<img src="./media/media/image57.png"
style="width:6.5in;height:3.78819in"
alt="A screenshot of a computer Description automatically generated" />

**Ensure the watchlist finishes uploading before moving to the next
lab.**

**We will use VMs in the next lab so that you can turn them on now or
before you begin the lab if you want to take a break.**

<span id="_Toc164711423" class="anchor"></span>**<u>Enabling Microsoft
Defender</u>**

**<u>Do you need your VMs to be on for this lab?</u>**

**YES (windows-vm, Linux-vm)**

Enable Microsoft Defender for Cloud for Log Analytics Workspace

<img src="./media/media/image58.png"
style="width:4.59206in;height:2.42521in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image59.png"
style="width:6.5in;height:3.97292in"
alt="A screenshot of a computer Description automatically generated" />

- Click on edit settings

<img src="./media/media/image60.png"
style="width:6.5in;height:2.92083in"
alt="A screenshot of a computer Description automatically generated" />

- Enable Defender Plans for VMs and SQL Instances on VMs

<img src="./media/media/image61.png"
style="width:6.5in;height:2.20972in"
alt="A screenshot of a computer Description automatically generated" />

- Enable Data Collection (All Events)

<img src="./media/media/image62.png"
style="width:6.5in;height:3.28056in"
alt="A screenshot of a computer Description automatically generated" />

Enable Microsoft Defender for Cloud for Subscription

- Click on edit settings

<img src="./media/media/image63.png"
style="width:6.5in;height:2.20208in"
alt="A screenshot of a computer Description automatically generated" />

- VMs, Storage Accounts, Key Vault, SQL Server

> <img src="./media/media/image64.png"
> style="width:6.5in;height:4.80833in"
> alt="A screenshot of a computer Description automatically generated" />

- Go to settings in the server row afterward

> <img src="./media/media/image65.png"
> style="width:6.5in;height:4.87778in"
> alt="A screenshot of a computer Description automatically generated" />

- Click on edit configuration and change to custom workspace to make
  sure it does not make its own workspace and logs from the actual VMS
  get sent to it.

> <img src="./media/media/image66.png"
> style="width:6.5in;height:5.34861in"
> alt="A screenshot of a computer Description automatically generated" />

- Click Apply, then continue afterward, and then save

> <img src="./media/media/image67.png"
> style="width:6.5in;height:5.14167in"
> alt="A screenshot of a computer Description automatically generated" />

- Make sure logs are being set to correct log analytics workspaces

Enable Microsoft Defender for Cloud Continuous Export in Environment
Settings

- Make sure to Export to the correct Log Analytics Workspace

- Do it for every single thing

> <img src="./media/media/image68.png"
> style="width:6.5in;height:5.14583in"
> alt="A screenshot of a computer Description automatically generated" /><img src="./media/media/image69.png"
> style="width:6.5in;height:5.01597in"
> alt="A screenshot of a computer Description automatically generated" />

Ensure MDC didn’t create another Log Analytics Workspace; delete it if
so.

We want to send everything to one Log Analytics Workspace for the sake
of the lab.

Looks good to continue!

<img src="./media/media/image70.png"
style="width:6.5in;height:1.77083in"
alt="A screenshot of a computer Description automatically generated" />

**We will continue using the VMs in the next lab, so you can leave them
on unless you’re going to take a break.**

# *<u>Enable Log Collection for VMs and Network Security Groups</u>*

It's a bit of a long lab. Grab yourself a drink 🙂☕︎

Create an Azure Storage Account (sacyberlab0x, name needs to be unique)

<img src="./media/media/image71.png"
style="width:5.81717in;height:2.43354in"
alt="A screenshot of a computer Description automatically generated" />

- MUST be in the same region as VMs

> <img src="./media/media/image72.png"
> style="width:6.5in;height:7.17083in"
> alt="A screenshot of a computer Description automatically generated" />
>
> <img src="./media/media/image73.png"
> style="width:6.26721in;height:7.45065in"
> alt="A screenshot of a computer Description automatically generated" />

- This will be used to store the NSG Flow Logs, which we are about to
  create

Enable Flow logs for both Network Security Groups (NSGs)

<img src="./media/media/image74.png"
style="width:4.29204in;height:2.10852in"
alt="A screenshot of a computer Description automatically generated" />

Click on the windows-vm-nsg

<img src="./media/media/image75.png"
style="width:6.41722in;height:1.26678in"
alt="A screenshot of a computer Description automatically generated" />

Click on nsg flow logs.

<img src="./media/media/image76.png"
style="width:2.08351in;height:7.43398in"
alt="A screenshot of a computer Description automatically generated" />

Click on Create Flow log.

<img src="./media/media/image77.png"
style="width:6.5in;height:3.34375in"
alt="A screenshot of a computer Description automatically generated" />

Click on select target resource and select network security group. That
way, you can enable a flow log for Linux vm at the same time. Should
redirect you if not click on resource afterwards.

<img src="./media/media/image78.png"
style="width:6.5in;height:6.04861in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image79.png"
style="width:3.62531in;height:2.52522in"
alt="A screenshot of a computer Description automatically generated" />

Confirm selection.

- Make sure to send to our Log Analytics Workspace

- If there is no storage account listed, it means it’s in a different
  region from your VMs, so you’ll need to create another storage account
  in the same region

<img src="./media/media/image80.png"
style="width:6.5in;height:6.77014in"
alt="A screenshot of a computer Description automatically generated" />

Click next Analytics make sure version 2 is clicked and enable traffic
analytics. Then click review+create and, look over everything, and
create.

<img src="./media/media/image81.png"
style="width:6.5in;height:4.18333in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image82.png"
style="width:5.97552in;height:5.08377in"
alt="A screenshot of a computer Description automatically generated" />

Configure Data Collection Rules within our Log Analytics Workspace

Click on agents

<img src="./media/media/image83.png"
style="width:2.20019in;height:5.20045in"
alt="A screenshot of a computer Description automatically generated" />

Click on data collection rules.

<img src="./media/media/image84.png"
style="width:6.5in;height:2.48472in"
alt="A screenshot of a computer error Description automatically generated" />

Usually, it would say 0 Windows or Linux connected, but mine is
different. Regardless, continue with the steps.

- Turn on VMs if they aren’t already on

- Configure Linux Data Sources (auth only)

  - Linux-vm-logs

- Configure Windows Data Sources (Application (information only),
  Security (All))

  - Windows-vm-logs

<img src="./media/media/image85.png"
style="width:3.31695in;height:2.6669in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image86.png"
style="width:6.5in;height:4.50347in"
alt="A screenshot of a computer Description automatically generated" />

Hit next. And add resources Linux and windows vm from rg cyber group.

<img src="./media/media/image87.png"
style="width:5.71716in;height:3.80033in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image88.png"
style="width:6.5in;height:3.81389in"
alt="A screenshot of a computer Description automatically generated" />

Hit next, and we will add data sources to the logs that will be logged
from the VMs.

- Select Linux, click only on LOG-AUTH, and leave at debug, which means
  collect everything. Click none for the rest of the options.

<img src="./media/media/image89.png"
style="width:6.5in;height:5.28681in"
alt="A screenshot of a computer Description automatically generated" />

Hit next at the bottom to add the data source.

<img src="./media/media/image90.png"
style="width:6.5in;height:5.49861in"
alt="A screenshot of a computer Description automatically generated" />

Next, add more data sources next is windows event logs.

<img src="./media/media/image91.png"
style="width:6.5in;height:7.23542in"
alt="A screenshot of a computer Description automatically generated" />

Click on next and add the same destination. Then, review and create.

Then, go back to the log analytics workspace. Click on the cyber lab,
then agents 🡪 data collection rules 🡪 Dcr-allvms.

<img src="./media/media/image92.png"
style="width:4.63373in;height:1.89183in"
alt="A screenshot of a computer Description automatically generated" />

Click on data sources.

<img src="./media/media/image93.png"
style="width:6.5in;height:2.68056in"
alt="A screenshot of a computer Description automatically generated" />

- Configure Special Windows Event Data Collection (Defender and Windows
  Firewall)

  - [<u>https://github.com/joshmadakor1/Cyber-Course-v2/blob/main/Special-Windows-Event-Data-Collection-Rules/Rules.txt</u>](https://github.com/joshmadakor1/Cyber-Course-v2/blob/main/Special-Windows-Event-Data-Collection-Rules/Rules.txt) 

Click on Windows and, change basic to custom, and input these rules.

// Windows Defender Malware Detection XPath Query

- Microsoft-Windows-Windows
  Defender/Operational!\*\[System\[(EventID=1116 or EventID=1117)\]\]

// Windows Firewall Tampering Detection XPath Query

- Microsoft-Windows-Windows Firewall With Advanced
  Security/Firewall!\*\[System\[(EventID=2003)\]\]

> <img src="./media/media/image94.png"
> style="width:6.5in;height:5.36458in"
> alt="A screenshot of a computer Description automatically generated" />
>
> Save and go back to check to make sure everything is applied
> correctly.

Manually install the Log Analytics Agent on both Windows VM and Linux
VM.

Go back to log analytics and into the cyber lab and agents tab. Click on
log analytics instructions.

You will log into the Windows VM, use the codes in your log analytics
workspace, and point back to the log analytics workspace to forward
them. This is just an extra precaution.

<img src="./media/media/image95.png"
style="width:6.5in;height:4.69653in"
alt="A screenshot of a computer Description automatically generated" />

Once you are in your Windows VM, copy the link for the download agent
and paste it into your VM. The Internet browser should automatically
download the installation and start it by following the prompts. Have
your ID and key ready. If its already installed remove (I did) and
reinstall with steps above and below.

<img src="./media/media/image96.png"
style="width:6.5in;height:4.50972in"
alt="A screenshot of a computer screen Description automatically generated" />

Next, all the way to install and finish the installation.

Go to the control panel, change the categories to large icons, and click
on Microsoft Monitoring Agent. Then click on Azure log analytics; it
should look like mine.

<img src="./media/media/image97.png"
style="width:6.5in;height:4.63889in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image98.png"
style="width:6.5in;height:4.42292in"
alt="A computer screen with a message Description automatically generated" />

Now, let's do Linux. SSH into your Linux VM and copy and paste the log
analytics instructions. It will do it all for you and should have a 0
error at the end. That’s it nothing else needs to be done move onto the
next steps.

<img src="./media/media/image99.png"
style="width:6.28388in;height:6.32555in"
alt="A screenshot of a computer security system Description automatically generated" />

<img src="./media/media/image100.png"
style="width:6.5in;height:3.66736in"
alt="A screenshot of a computer Description automatically generated" />

**Keep checking / refreshing the Log Analytics agents tab and ensure the
VMs show up there**

**Begin querying Log Analytics for logs from the VMs and NSGs; do not
move on from this lab until you see logs from all three sources or at
least the Linux/windows logs:**

- **Syslog (Linux):**

- **SecurityEvent (windows)**

- **AzureNetworkAnalytics_CL (Network Security Groups/NSGs)**

Linux

<img src="./media/media/image101.png"
style="width:6.5in;height:1.90278in"
alt="A screenshot of a computer Description automatically generated" />

Windows:

<img src="./media/media/image102.png"
style="width:6.5in;height:1.49306in"
alt="A screenshot of a computer Description automatically generated" />

NSGs:

<img src="./media/media/image103.png"
style="width:6.5in;height:2.94653in"
alt="A screenshot of a computer Description automatically generated" />

When the logs are coming in, test generating a couple of logs (failed
logons for Windows/Linux) and observe them in Log Analytics

\*note\* Make sure to give it time if one or the other VM is not
producing logs.

\- make sure the NSG flow interval is 10 min, not the 1hr default

\- make sure to add the VMs to resources if not done on its own.

\- manually add the agents to the VMs to really be sure they are on
there

\- make sure syslog is running on linux VMs

Visual Recap: [<u>Logging and Monitoring: Enable MDC and Configure Log
Collection for Virtual
Machines</u>](https://docs.google.com/presentation/d/1Sd71Zm_J8PY06L3_YzoOpvctenhFFxJe9wB_OwA-MVk/edit#slide=id.g2191f5eb6b4_0_0)

**As long as you see logs coming in, you can shut down your Virtual
Machines to save money.**

# *<u>Kusto Query Language (KQL)</u>*

<img src="./media/media/image104.png"
style="width:6.5in;height:1.87847in"
alt="A screenshot of a computer Description automatically generated" />
<img src="./media/media/image105.png"
style="width:6.5in;height:1.31389in"
alt="A screenshot of a computer Description automatically generated" />
<img src="./media/media/image106.png"
style="width:1.60847in;height:2.10852in"
alt="A white paper with black text Description automatically generated" />

<https://github.com/joshmadakor1/Cyber-Course/blob/main/KQL-Query-Cheat-Sheet.md>

# *<u>Tenant Level Logging</u>*

**<u>Do you need your VMs to be on for this lab?</u>**

**NO (All VMs)**

**Check your Subscription’s [<u>Cost
Analysis</u>](https://portal.azure.com/#view/Microsoft_Azure_CostManagement/Menu/~/costanalysis/openedBy/AzurePortal)**

**<u>Admin Mode (pretend you are normal admin):</u>**

**<u>Setup logging for Azure Active Directory (aka Microsoft Entra ID)
and generate some logs</u>**

Create Diagnostic Settings to ingest Azure AD Logs

- Enable Audit Logs and Signin Logs for Azure AD

<img src="./media/media/image107.png"
style="width:6.5in;height:5.52639in"
alt="A screenshot of a computer Description automatically generated" />
<img src="./media/media/image108.png"
style="width:6.5in;height:3.70556in"
alt="A screenshot of a computer Description automatically generated" />

Check Log Analytics Workspace that the tables have been created:
“AuditLogs” “SigninLogs”

<img src="./media/media/image109.png"
style="width:6.5in;height:3.92431in"
alt="A screenshot of a computer Description automatically generated" />

You can also check on the logs tab and see if the table has been created
if it doesn’t show up on tables section. If It wasn’t created it’d have
a red error message so it’s there in the image below just has not gotten
any logs yet. <img src="./media/media/image110.png"
style="width:6.5in;height:3.29167in"
alt="A screenshot of a computer Description automatically generated" />

Create a dummy user, username “dummy_user”

Log in with it once (incognito window)

Assign dummy_user the Role of Global Administrator

Back in Active Directory.

<img src="./media/media/image111.png"
style="width:2.05018in;height:3.0336in"
alt="A screenshot of a computer Description automatically generated" />
<img src="./media/media/image112.png"
style="width:4.9671in;height:1.90017in"
alt="A screenshot of a computer Description automatically generated" />
<img src="./media/media/image113.png"
style="width:5.60049in;height:7.41731in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image114.png"
style="width:4.83375in;height:7.53399in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image115.png"
style="width:6.5in;height:3.67014in" />

<img src="./media/media/image116.png"
style="width:5.90885in;height:7.3423in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image117.png"
style="width:6.5in;height:2.44861in"
alt="A screenshot of a computer Description automatically generated" />

Delete dummy_user

<img src="./media/media/image118.png"
style="width:5.12544in;height:1.92517in"
alt="A screenshot of a computer Description automatically generated" />

Observe Audit Logs logs in Log Analytics Workspace.

<img src="./media/media/image119.png"
style="width:6.5in;height:4.34097in"
alt="A screenshot of a computer Description automatically generated" />

Rebuild/understand this query (Note: Some people’s tenant Global
Administrator can show up as either Company Administrator or “Global
Administrator.” Microsoft did this for some reason):

AuditLogs

\| where OperationName == "Add member to role" and Result == "success"

\| where TargetResources\[0\].modifiedProperties\[1\].newValue ==
'"Global Administrator"' or
TargetResources\[0\].modifiedProperties\[1\].newValue == '"Company
Administrator"' 

\| order by TimeGenerated desc

\| project TimeGenerated, OperationName, AssignedRole =
TargetResources\[0\].modifiedProperties\[1\].newValue, Status = Result,
TargetResources

<img src="./media/media/image120.png"
style="width:6.5in;height:4.25278in"
alt="A screenshot of a computer Description automatically generated" />

**<u>Attacker Mode (pretend you are an attacker):</u>**

**<u>Simulate Brute Force Attack against Azure Active Directory (aka
Microsoft Entra ID)</u>**

Create the “attacker” user if it does not exist already

<img src="./media/media/image121.png"
style="width:2.39187in;height:7.44231in"
alt="A screenshot of a computer Description automatically generated" />

- Produce 10-11 failed Logins with the portal in incognito mode

- <img src="./media/media/image122.png"
  style="width:6.5in;height:5.27222in"
  alt="A screenshot of a computer Description automatically generated" />

**<u>Admin Mode (pretend you are normal admin):</u>**

Observe SigninLogs in Log Analytics Workspace. It may take a while; give
it time.

Rebuild/understand this query:

SigninLogs

\| where ResultDescription == "Invalid username or password or Invalid
on-premise username or password."

\| extend location = parse_json(LocationDetails)

\| extend City = location.city, State = location.state, Country =
location.countryOrRegion, Latitude = location.geoCoordinates.latitude,
Longitude = location.geoCoordinates.longitude

\| project TimeGenerated, ResultDescription, UserPrincipalName,
AppDisplayName, IPAddress, IPAddressFromResourceProvider, City, State,
Country, Latitude, Longitude

<img src="./media/media/image123.png" style="width:6.5in;height:3.775in"
alt="A screenshot of a computer Description automatically generated" />

**We don’t need VMs for the next video, so leave/turn them off if you
want to save money.**

Global Administrator Break-Glass account in case your main one gets
locked out. Just for extra precaution.

**<u>Do you need your VMs to be on for this lab?</u>**

**NO (All VMs)**

Create a backup account in Azure Active Directory (aka Microsoft Entra
ID) and assign it Global Administrator. 

<img src="./media/media/image124.png"
style="width:5.56715in;height:7.58399in"
alt="A screenshot of a computer Description automatically generated" />

Log into it once so you can change the password.

Make note of the Accounts login (UPN) and password (consider a password
manager)

**We don’t need VMs for the next video, so leave/turn them off if you
want to save money.**

# *<u>Next Will be doing Subscription Level Logging (Activity Log)</u>* 

# <img src="./media/media/image1.png"
style="width:6.10485in;height:3.40267in"
alt="A screenshot of a computer Description automatically generated" /> 

<img src="./media/media/image125.png"
style="width:6.5in;height:3.11628in"
alt="A diagram of a log book Description automatically generated" />

**<u>Do you need your VMs to be on for this lab?</u>**

**NO (All VMs)**

**Check your Subscription’s [<u>Cost
Analysis</u>](https://portal.azure.com/#view/Microsoft_Azure_CostManagement/Menu/~/costanalysis/openedBy/AzurePortal)**

Export Azure Activity Logs to Log Analytics Workspace

<img src="./media/media/image126.png"
style="width:5.85051in;height:1.83349in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image127.png"
style="width:6.5in;height:3.92917in"
alt="A screenshot of a computer Description automatically generated" />

Create diagnostic setting

<img src="./media/media/image128.png"
style="width:4.06702in;height:4.95043in"
alt="A screenshot of a computer Description automatically generated" />
<img src="./media/media/image129.png"
style="width:6.5in;height:4.12778in"
alt="A screenshot of a computer Description automatically generated" />

Generate some logs:

Create a new Resource Group named “Scratch-Resource-Group.”

Create another new Resource Group named
“Critical-Infrastructure-Wastewater.”

<img src="./media/media/image130.png"
style="width:3.80033in;height:3.04193in"
alt="A screenshot of a web page Description automatically generated" />
<img src="./media/media/image131.png"
style="width:3.88367in;height:2.37521in"
alt="A screenshot of a computer Description automatically generated" />

Delete both “Scratch-Resource-Group” and “Critical-Infrastructure” **(DO
NOT ACCIDENTALLY DELETE YOUR LAB RESOURCE GROUP)**

<img src="./media/media/image132.png"
style="width:2.87525in;height:2.40854in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image133.png"
style="width:6.5in;height:4.65972in"
alt="A screenshot of a computer Description automatically generated" />

Test Lab Queries

// Querying for the deletion of critical Resource Groups

AzureActivity

\| where ResourceGroup starts with "Critical-Infrastructure-"

\| order by TimeGenerated

// Querying for changes to network security groups

AzureActivity

\| where OperationNameValue ==
"MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/SECURITYRULES/WRITE"

// Optionally, specific Resource Groups:

// \| where ResourceGroup in ("resource-group-1", "resource-group-2") 

\| order by TimeGenerated

// Deletion activities within a certain timespan

AzureActivity

\| where OperationNameValue endswith "DELETE"

\| where ActivityStatusValue == "Success"

\| where TimeGenerated \> ago(30m)

\| order by TimeGenerated

// From Microsoft Defender for Cloud Security Events

AzureActivity

\| where CategoryValue == "Security"

// Just stuff happening on the Management Plane

AzureActivity

\| where CategoryValue != "Administrative"

**We don’t need VMs for the next video, so leave/turn them off if you
want to save money.**

# *<u>Resource Level Logging (Diagnostics settings)</u>*

<img src="./media/media/image134.png"
style="width:6.5in;height:3.59375in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image135.png"
style="width:6.5in;height:3.45556in"
alt="A diagram of a logistic log Description automatically generated" />

**<u>Do you need your VMs to be on for this lab?</u>**

**NO (All VMs)**

**<u>Configure Logging for Azure Storage</u>**

Configure logging for your storage account by enabling diagnostic
settings for blob storage

- Collect All Logs and Metrics (metrics will record auth failures) and
  send to your Log Analytics Workspace instance

> <img src="./media/media/image136.png"
> style="width:4.29204in;height:3.15861in"
> alt="A screenshot of a computer Description automatically generated" /><img src="./media/media/image137.png"
> style="width:6.5in;height:3.04653in"
> alt="A screenshot of a computer Description automatically generated" />
>
> <img src="./media/media/image138.png"
> style="width:6.5in;height:3.27986in" />
>
> <img src="./media/media/image139.png"
> style="width:6.5in;height:3.81181in"
> alt="A screenshot of a computer Description automatically generated" />

**<u>Configure Logging for Key Vault</u>**

Create a Key Vault Instance

Configure logging for your Key Vault instance by enabling diagnostic
settings

- Collect the audit log and send to you Log Analytics Workspace instance

- Add a secret to Key Vault called “Tenant-Global-Admin-Password” with a
  made-up password

- Observe the key in key vault

> <img src="./media/media/image140.png"
> style="width:6.5in;height:2.70694in"
> alt="A screenshot of a computer Description automatically generated" />
>
> <img src="./media/media/image141.png"
> style="width:4.27537in;height:2.61689in"
> alt="A key in a circle with text Description automatically generated" />
>
> <img src="./media/media/image142.png"
> style="width:6.31721in;height:7.08395in"
> alt="A screenshot of a computer screen Description automatically generated" />
>
> <img src="./media/media/image143.png"
> style="width:5.17545in;height:7.20896in"
> alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image144.png"
style="width:4.15869in;height:7.14229in"
alt="A screenshot of a computer Description automatically generated" />

Go to access policies In key vault that was created and make sure you
the user has all the proper access that is needed to generate logs.

<img src="./media/media/image146.png"
style="width:4.6754in;height:2.50855in"
alt="A screenshot of a computer screen Description automatically generated" />

<img src="./media/media/image147.png"
style="width:6.47556in;height:7.21729in"
alt="A screenshot of a computer Description automatically generated" />

Next lets create a secret.

<img src="./media/media/image148.png"
style="width:4.17536in;height:3.83367in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image149.png"
style="width:4.01701in;height:7.3173in"
alt="A screenshot of a computer Description automatically generated" />

Enable Diagnostic Settings in key vault

<img src="./media/media/image150.png"
style="width:5.53381in;height:4.52539in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image151.png"
style="width:6.5in;height:4.47222in"
alt="A screenshot of a computer Description automatically generated" />

Create another secret just to make sure some logs are being generated.

Generate a test container in Storage account to create log as well.

<img src="./media/media/image152.png"
style="width:5.20878in;height:4.08369in"
alt="A screenshot of a computer Description automatically generated" />

Click on the test container and upload a dummy Txt file to generate
logs.

Generate some Logs for the Storage Account and Key Vault

- Generate some Logs for Azure Storage (read some blobs/files)

- Observe the Logs (they may take a moment to appear) -  [<u>KQL Query
  Cheat
  Sheet</u>](https://github.com/joshmadakor1/Cyber-Course/blob/main/KQL-Query-Cheat-Sheet.md)

**Storage Account Test Logs**

// Authorization Error

StorageBlobLogs 

\| where MetricResponseType endswith "Error" 

\| where StatusText == "AuthorizationPermissionMismatch"

\| order by TimeGenerated asc

<img src="./media/media/image153.png"
style="width:6.5in;height:2.31875in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image154.png"
style="width:4.56706in;height:4.55873in"
alt="A screenshot of a computer Description automatically generated" />

// Reading a bunch of blobs

StorageBlobLogs

\| where OperationName == "GetBlob"

//Deleting a bunch of blobs (in a short time period)

StorageBlobLogs \| where OperationName == "DeleteBlob"

\| where TimeGenerated \> ago(24h)

//Putting a bunch of blobs (in a short time period) 

StorageBlobLogs \| where OperationName == "PutBlob"

\| where TimeGenerated \> ago(24h)

//Copying a bunch of blobs (in a short time period)

StorageBlobLogs \| where OperationName == "CopyBlob"

\| where TimeGenerated \> ago(24h)

**Key Vault Test Logs**

// List out Secrets

AzureDiagnostics

\| where ResourceProvider == "MICROSOFT.KEYVAULT"

\| where OperationName == "SecretList"

// Attempt to view passwords that don't exist

AzureDiagnostics

\| where ResourceProvider == "MICROSOFT.KEYVAULT"

\| where OperationName == "SecretGet"

\| where ResultSignature == "Not Found"

// Viewing an actual existing password

AzureDiagnostics

\| where ResourceProvider == "MICROSOFT.KEYVAULT"

\| where OperationName == "SecretGet"

\| where ResultSignature == "OK"

// Viewing a specific existing password

let CRITICAL_PASSWORD_NAME = "Tenant-Global-Admin-Password";

AzureDiagnostics

\| where ResourceProvider == "MICROSOFT.KEYVAULT"

\| where OperationName == "SecretGet"

\| where id_s contains CRITICAL_PASSWORD_NAME

// Updating a password Success

AzureDiagnostics

\| where ResourceProvider == "MICROSOFT.KEYVAULT" 

\| where OperationName == "SecretSet"

// Updating a specific existing password Success

let CRITICAL_PASSWORD_NAME = "Tenant-Global-Admin-Password";

AzureDiagnostics

\| where ResourceProvider == "MICROSOFT.KEYVAULT" 

\| where OperationName == "SecretSet"

\| where id_s endswith CRITICAL_PASSWORD_NAME

\| where TimeGenerated \> ago(2h)

// Failed access attempts

AzureDiagnostics

\| where ResourceProvider == "MICROSOFT.KEYVAULT" 

\| where ResultSignature == "Unauthorized"

**NOTE! In the next lab, we’re going to start onboarding logs to our
SIEM, so it may be a good idea to turn your VMs on now (both windows-vm
and linux-vm) and perhaps even let them run overnight to generate some
logs from people trying to break into them over the internet.**

# *<u>Microsoft Sentinel Build</u>*

<img src="./media/media/image155.png"
style="width:6.5in;height:3.41111in"
alt="A white paper with black text Description automatically generated" />

<img src="./media/media/image156.png"
style="width:6.5in;height:2.59167in"
alt="A screenshot of a computer error Description automatically generated" />

**<u>Do you need your VMs to be on for this lab?</u>**

**YES (windows-vm, Linux-vm)**

Check your Subscription’s [Cost
Analysis](https://portal.azure.com/#view/Microsoft_Azure_CostManagement/Menu/~/costanalysis/openedBy/AzurePortal)

FYI:

We are going to create 4 different workbooks in Sentinel that show
different types of malicious traffic from around the world, targeting
our resources.

We will use pre-built JSON maps to reduce the number of
errors/questions, but will explain the process.

Normally making maps like this takes a lot of time and tweaking.

<img src="./media/media/image157.png"
style="width:6.5in;height:3.12083in"
alt="Malicious Inbound Flows Map" />

——————————————————————————————————————

Ref: [JSON
Files](https://github.com/joshmadakor1/Cyber-Course-V2/tree/main/Sentinel-Maps(JSON)) -
Remember, Sentinel uses our Log Analytics Workspace, where we ingested
the logs

- You can also just copy and paste the info into Microsoft Sentinel

- Maps is created in work books so click on workbooks and add workbook.

> <img src="./media/media/image158.png"
> style="width:5.83883in;height:3.35421in"
> alt="A screenshot of a computer Description automatically generated" />Click
> on edit and remove the elements.
>
> <img src="./media/media/image159.png"
> style="width:5.93933in;height:4.65565in"
> alt="A screenshot of a computer Description automatically generated" />

- Then click on add at the bottom. And click on query on the drop down.
  Then, on advanced editor.

> <img src="./media/media/image160.png"
> style="width:5.74136in;height:4.62682in"
> alt="A screenshot of a computer Description automatically generated" />

Add the Linux files into it by copying and pasting. And click done
editing.

NOTE= geoip watchlist schema aka the column names have to match the json
files to query correctly. Error may occure because of this difference in
the files.

Change Countryname and Cityname on Watchlist to not have a \_ Underscore
or change the json sentinel files to have the underscores.

Within Azure Sentinel, **<u>first observe the Data Connectors</u>**,
then do the following:

- Use **windows-rdp-auth-fail.json** to create the “<u>Windows RDP/SMB
  Authentication Failures</u>” map

- Use **Linux-ssh-auth-fail.json** to create the “<u>Linux SSH
  Authentication Failures</u>” map

- Use **MySQL-auth-fail.json** to create the “<u>MS SQL Server
  Authentication Failures</u>” map

- Use **nsg-malicious-allowed-in.json** to create the “<u>NSG Allowed
  Malicious Inbound Flows</u>” map

- Example of what it should look like below, and you can check the query
  that is being used by sentinel to output that data by clicking edit
  top right 🡪 edit bottom right and then copying and pasting that back
  into log analytics to check out more info.

<img src="./media/media/image161.png"
style="width:6.5in;height:5.95625in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image162.png"
style="width:6.5in;height:3.99931in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image163.png"
style="width:6.5in;height:6.01458in"
alt="A screenshot of a computer Description automatically generated" />

Observe any pre-existing malicious attack traffic on these maps from the
Internet.

Ensure an appropriate time frame is being selected (30 days)

——————————————————————————————————————

**If it’s been 24 hours since you created the resources being tracked on
this map and you don’t see traffic to them, make sure of the
following:**

- First, generate traffic on your own to see if any logs show up

- Ensure both VMs are on

- Ensure Microsoft Defender for Cloud and the Data Collection Rules are
  configured correctly to collect logs from the VMs (from section:
  Logging and Monitoring: Enable MDC and Configure Log Collection for
  Virtual Machines)

- Ensure Logging is correctly configured for MS SQL Server (from
  section: Azure Intro: Creating our Subscription and First Resources)

- If NSG FLow Logs are empty, ensure they are configured correctly (from
  section: Logging and Monitoring: Enable MDC and Configure Log
  Collection for Virtual Machines)

- Alternatively, you can skip ahead to the “Azure Sentinel: Attack
  Traffic Generation” section to generate some traffic, but we need to
  make sure logging is configured correctly and showing up before that
  will work.

**We’re going to continue to work with logs from the VMs, so it may be a
good idea to leave your VMs on (unless you’re going to go to bed or
something and want to maximize your savings)**

# *<u>Manual Alert Creation</u>*

**<u>Do you need your VMs to be on for this lab?</u>**

**YES (windows-vm)**

**Check your Subscription’s [Cost
Analysis](https://portal.azure.com/#view/Microsoft_Azure_CostManagement/Menu/~/costanalysis/openedBy/AzurePortal)**

Within Azure Sentinel, browse to “Analytics” -\> “Active Rules”

- Manually create the “TEST: Brute Force ATTEMPT - Windows” Rule

- <img src="./media/media/image164.png"
  style="width:6.5in;height:2.67222in"
  alt="A screenshot of a computer Description automatically generated" />

> <img src="./media/media/image165.png"
> style="width:6.5in;height:8.81528in"
> alt="A screenshot of a computer Description automatically generated" /><img src="./media/media/image166.png"
> style="width:6.5in;height:7.65486in"
> alt="A screenshot of a computer screen Description automatically generated" />
> <img src="./media/media/image167.png"
> style="width:6.5in;height:8.00694in"
> alt="A screenshot of a computer Description automatically generated" />
>
> <img src="./media/media/image168.png"
> style="width:6.5in;height:6.32292in"
> alt="A screenshot of a computer Description automatically generated" />
>
> <img src="./media/media/image169.png"
> style="width:6.5in;height:6.90486in"
> alt="A screenshot of a computer Description automatically generated" />

- **SecurityEvent  
  \| where EventID == 4625  
  \| where TimeGenerated \> ago(60m)  
  \| summarize FailureCount = count() by AttackerIP = IpAddress,
  EventID, Activity, DestinationHostName = Computer  
  \| where FailureCount \>= 10**

<!-- -->

- Try to trigger it by logging into windows vm atleast ten times fail
  the login.

- Then look for it in incidents in sentienel and you can click on
  invistigate on lower right side panel. Gives a visual representation
  of the attack.

> <img src="./media/media/image170.png"
> style="width:6.5in;height:2.98889in"
> alt="A screenshot of a computer Description automatically generated" />

- Delete the Rule when finished

**We’re going to continue to work with logs from the VMs, so it may be a
good idea to leave your VMs on (unless you’re going to go to bed or
something and want to maximize your savings)**

<span id="_Toc164711432" class="anchor"></span>***<u>Automatic Alert
Import</u>***

**<u>Do you need your VMs to be on for this lab?</u>**

**YES (windows-vm, Linux-vm)**

Import all of our [Sentinel Analytics
Rules](https://github.com/joshmadakor1/Cyber-Course-V2/tree/main/Sentinel-Analytics-Rules)

<img src="./media/media/image171.png"
style="width:6.5in;height:4.13958in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image172.png"
style="width:6.5in;height:3.13611in"
alt="A screenshot of a computer Description automatically generated" />

Select the “CUSTOM: Brute Force SUCCESS - Windows” Alerts and “Edit” it:

Copy the query and fully inspect/dissect it in Log Analytics Workspace.

User [ChatGPT](https://chat.openai.com/chat) to understand the rule
further:

<img src="./media/media/image173.png" style="width:6in;height:4.59293in"
alt="A screenshot of a computer Description automatically generated" />

**(This even works on complicated queries such as “CUSTOM: Brute Force
SUCCESS - Azure Active Directory”)**

Do this for the remaining “CUSTOM” Analytics Rules. Make sure you
understand.

**<u>Note:</u>**

In the next section, we will generate traffic from “attack-vm” to
trigger some of these rules.

Many of them will be triggered on their own from live Internet traffic.

**We’re going to continue to work with logs from the VMs, so it may be a
good idea to leave your VMs on (unless you’re going to go to bed or
something and want to maximize your savings)**

# *<u>Understanding and Triggering Sentinel</u>*

**<u>Do you need your VMs to be on for this lab?</u>**

**YES (windows-vm, Linux-vm, attack-vm)**

**<u>When triggering an alert, you can go to Analytics in Sentinel, copy
the rule query, and put that into log analytics to see the results
quicker before they show up as an incident.</u>**

<img src="./media/media/image174.png"
style="width:6.5in;height:2.92361in"
alt="A screenshot of a computer Description automatically generated" />

**<u>Trigger AAD Brute Force Success </u>**

- (from within attack-vm) Simulate brute force success against Azure AD
  with your attacker account:

  - In an incognito windows, open portal.azure.com and fail 10-11 logins
    in a row, followed by a successful login

    - Bonus points if you want to try with PowerShell:
      [AAD-Brute-Force-Success-Simulator.ps1](https://github.com/joshmadakor1/Cyber-Course-V2/edit/main/Attack-Scripts/AAD-Brute-Force-Success-Simulator.ps1)

**<u>Trigger MSSQL Brute Force Attempt </u>**

- (from within attack-vm) Open SSMS and simulate brute force attempt
  against your SQL Server by attempting to log into it 10-11 times

  - Possible to do with PowerShell, not required
    [SQL-Brute-Force-Simulator.ps1](https://github.com/joshmadakor1/Cyber-Course-V2/blob/main/Attack-Scripts/SQL-Brute-Force-Simulator.ps1)

<img src="./media/media/image175.png"
style="width:6.5in;height:2.71597in"
alt="A screenshot of a computer Description automatically generated" />

**<u>Trigger Malware Outbreak</u>**

(from within windows-vm) Generate a Malware alert by using an EICAR file

- [Malware-Generator-EICAR.ps1](https://github.com/joshmadakor1/Cyber-Course/blob/main/Attack-Scripts/Malware-Generator-EICAR.ps1)
  **(Do from within Windows VM)**

  - (this can be done manually by creating a text file with the EICAR
    string in it)

<img src="./media/media/image176.png"
style="width:6.5in;height:3.28056in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image177.png"
style="width:6.5in;height:3.01528in"
alt="A screenshot of a computer Description automatically generated" />

You can also see the alerts within the windows VM just to get an overall
look of how its displayed on there. Sentinel query Windows for this info
and displays it in logs.

<img src="./media/media/image178.png"
style="width:6.5in;height:4.10833in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image179.png"
style="width:6.5in;height:1.74028in"
alt="A screenshot of a computer Description automatically generated" />

**<u>Trigger Possible Privilege Escalation (AKV Critical Credential
Retrieval or Update) </u>**

- Manually Read Key Vault Secret “Tenant-Global-Admin-Password” in the
  portal and observe the incidents

- <img src="./media/media/image180.png"
  style="width:6.5in;height:2.37083in"
  alt="A screenshot of a computer Description automatically generated" />

> <img src="./media/media/image181.png"
> style="width:6.5in;height:5.87569in"
> alt="A screenshot of a computer Description automatically generated" />
>
> This rule exists to be able to investigate why and who has looked at
> the secrets even if its within the organization.

**<u>Trigger Windows Host Firewall Tampering</u>**

- Manually Enable and Disable the windows-vm Firewall and observe the
  incidents

**<u>Trigger Excessive Password Resets</u>**

- Manually Trigger excessive password resets ([KQL-Cheat-Sheet.
  md](https://github.com/joshmadakor1/Cyber-Course-V2/blob/main/KQL-Query-Cheat-Sheet.md))
  and observe the incidents by resetting a users’ password in the portal
  10-11 times

  - Make a new user and just reset the password in the portal.

> <img src="./media/media/image182.png"
> style="width:6.5in;height:2.69375in"
> alt="A screenshot of a computer Description automatically generated" />
>
> <img src="./media/media/image183.png"
> style="width:6.5in;height:2.47569in"
> alt="A screenshot of a computer Description automatically generated" />

Optional: attempt to trigger the rest of the customs rules to make sure
they work

Note:

It does take a bit of time for the logs to show up in the Log Analytics
Workspace.

If you want to trigger Brute Force attempts for Linux and RDP, simply
fail logging into these several times, but I assume the internet is
doing a good job of that already.

**We’re going to continue to work with logs from the VMs, so it may be a
good idea to leave your VMs on (unless you’re going to go to bed or
something and want to maximize your savings)**

**However, you do not need the attack VM from this point, so feel free
to turn it off or delete it.**

# *<u>Run the Insecure Environment for 24hrs and Capture Analytics</u>*

Spreadsheet: [BEFORE AND
AFTER](https://docs.google.com/spreadsheets/d/1MWJuaa1OcYXojxhjwDH_gtEV3kDvrf2RPgRi10XcVQQ/edit#gid=0)

Make sure all of the following queries/tables return something:

- SecurityEvent

- Syslog

- SecurityAlert

- SecurityIncident

- AzureNetworkAnalytics_CL 

Let the environment sit for 24 hours as is.

When we come back, we will count all the alerts/etc that happened in the
last 24 hours.

Secure our environment.

Let it sit for another 24 hours, secure again.

Come back and count the alerts/etc, then make a comparison between the
two.

<img src="./media/media/image184.png"
style="width:6.5in;height:6.03681in"
alt="A map of the world with green dots and red circles Description automatically generated" />

<img src="./media/media/image185.png" style="width:6.5in;height:5.75in"
alt="A screenshot of a computer screen Description automatically generated" />

<img src="./media/media/image186.png"
style="width:6.5in;height:5.80903in"
alt="A screenshot of a computer Description automatically generated" />

Incident 1 - Brute Force Success

Windows - Working incidents and incidents Response

**Check your Subscription’s [Cost
Analysis](https://portal.azure.com/#view/Microsoft_Azure_CostManagement/Menu/~/costanalysis/openedBy/AzurePortal)**

Work on the incidents being generated within Azure Sentinel in
accordance with the

NIST 800-61 Incident Management Lifecycle. Make use of the provided Pl.

<img src="./media/media/image187.png" style="width:6in;height:3.12778in"
alt="A diagram of a recovery process Description automatically generated with medium confidence" />

**Step 1: Preparation**

- (We initiated this already by ingesting all of the logs into Log
  Analytics Workspace and Sentinel and configuring alert rules)

**Step 2: Detection & Analysis (You may have different
alerts/incidents)**

1.  Set Severity, Status, Owner

<img src="./media/media/image188.png"
style="width:6.5in;height:4.04583in"
alt="A screenshot of a computer Description automatically generated" />

2.  View Full Details (New Experience)

> <img src="./media/media/image189.png"
> style="width:6.5in;height:4.71319in"
> alt="A screenshot of a computer Description automatically generated" />

3.  Observe the Activity Log (for history of incident)

4.  Observe Entities and Incident Timelines (are they doing anything
    else?)

5.  “Investigate” the incident and continue trying to determine the
    scope

<img src="./media/media/image190.png"
style="width:6.5in;height:4.46875in"
alt="A screenshot of a computer Description automatically generated" />

6.  Inspect the entities and see if there are any related events.

    1.  Below, we see the VM has other related incidents by the visual
        dotted lines.

<img src="./media/media/image191.png"
style="width:5.37575in;height:5.47993in"
alt="A screenshot of a computer Description automatically generated" />

The image below shows the vm has been part of a whole bunch of other
attacks.

<img src="./media/media/image192.png"
style="width:6.5in;height:3.79167in"
alt="A screenshot of a computer Description automatically generated" />

> So, to clarify, the IP address is the attacker, the Windows icon is
> the victim, and the attached incidents are the attacks that have been
> done against it.

7.  Determine legitimacy of the incident (True Positive, False Positive,
    etc.)

    1.  You can do that by looking at the analytics rule just to query
        around and see what exactly was done.

    2.  Click on the articles and read to get a better idea

8.  If True Positive, continue, if False positive, close it out

    1.  <img src="./media/media/image193.png"
        style="width:3.65676in;height:7.25101in"
        alt="A screenshot of a computer error message Description automatically generated" />

**Step 3: Containment, Eradication, and Recovery**

- Use the simple [Incident Response
  PlayBook](https://docs.google.com/document/d/1EQ5MzN95POLaRIMulYg3PIH3UGHtDNcGdkFvgOXyEXQ/edit)

- <img src="./media/media/image194.png"
  style="width:6.5in;height:4.15208in"
  alt="A screenshot of a computer error Description automatically generated" />

**Step 4: Document Findings/Info and Clouse out the Incident in
Sentinel**

I hardened the NSG of all the VMs. I put my IP so any other IP will be
denied.

<img src="./media/media/image195.png"
style="width:6.5in;height:4.59583in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image196.png"
style="width:6.5in;height:4.64167in"
alt="A screenshot of a computer Description automatically generated" />

Repeat these steps for all the other Incidents. This is a practice to
get an idea of what a SOC analyst does.

# *<u>Regulatory Compliance (Enable NIST 800-53) (New)</u>*

**Check your Subscription’s [Cost
Analysis](https://portal.azure.com/#view/Microsoft_Azure_CostManagement/Menu/~/costanalysis/openedBy/AzurePortal)**

Inspect MDC Secure Score
([Ref](https://learn.microsoft.com/en-us/microsoft-365/security/defender/microsoft-secure-score?view=o365-worldwide))

<img src="./media/media/image197.png"
style="width:6.5in;height:3.84028in"
alt="A screenshot of a computer Description automatically generated" />

Inspect MDC Recommendations

<img src="./media/media/image198.png"
style="width:6.5in;height:4.91181in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image199.png"
style="width:6.5in;height:5.21806in"
alt="A screenshot of a computer Description automatically generated" />

ENABLE MDC Regulatory Compliance (this takes  a while to appear) 

- NIST 800-53
  ([Ref](https://csrc.nist.gov/projects/cprt/catalog#/cprt/framework/version/SP_800_53_5_1_0/home))
  (Slides Refresher: [NIST 800-53
  Slides](https://docs.google.com/presentation/d/1JeK0tJWWjHDwJ0_UZVHw5IqM0BLkbPC89yHSwFdY9jE/edit))
  Compare NIST site with MDC recommendations

> <img src="./media/media/image200.png"
> style="width:4.02139in;height:8.93875in"
> alt="A screenshot of a computer Description automatically generated" />
>
> <img src="./media/media/image201.png"
> style="width:5.58411in;height:4.03181in"
> alt="A screenshot of a computer Description automatically generated" />
>
> <img src="./media/media/image202.png"
> style="width:6.5in;height:1.30833in"
> alt="A screenshot of a video Description automatically generated" />
>
> <img src="./media/media/image203.png"
> style="width:6.5in;height:1.49236in"
> alt="A screenshot of a computer Description automatically generated" />

Go back to Microsoft Defender for cloud and click on regulatory
compliance to see that 800-53 security and privacy controls have been
implemented. IT MAY TAKE HOURS TO SHOW.

- In the next video, we will Implement SC-7 (this could potentially take
  forever to implement in real life due to CAB (change
  a<https://csrc.nist.gov/projects/cprt/catalog#/cprt/framework/version/SP_800_53_5_1_0/home>dvisory
  board) and change management meetings/testing, but we can just do it. 

Some good ideas for additional portfolio projects might be walkthroughs
on how to remediate some of the other findings from MDC Recommendations
or the NIST 800-53 Regulatory Compliance Policy 🙂.

For Example: How-To’s for enabling MFA in Azure, implementing Azure
Firewall, etc.

**You can leave your VMs off.**

# *<u>Azure Private Link- Firewall for Resources (NIST SC-7)</u>*

**<u>Do you need your VMs to be on for this lab?</u>**

**Yes (windows-vm)**

**p**

Inspect MDC Regulatory Compliance (Available and Implemented)

- NIST 800-53
  ([Ref](https://csrc.nist.gov/Projects/risk-management/sp800-53-controls/release-search#/families?version=5.1))

  - We will Implement SC-7, not all the options, just some of them.

<img src="./media/media/image204.png"
style="width:6.5in;height:4.69583in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image205.png"
style="width:6.5in;height:4.62917in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image206.png"
style="width:6.5in;height:2.83889in"
alt="A screenshot of a computer Description automatically generated" />

Configure Azure Private Link and Firewall for your Azure Key Vault
instance

- Ensure you use the same region and VNet the rest of your VMs are in

<img src="./media/media/image207.png"
style="width:6.5in;height:5.51736in"
alt="A screenshot of a computer Description automatically generated" /><img src="./media/media/image208.png"
style="width:6.5in;height:2.84375in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image209.png"
style="width:6.5in;height:3.75208in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image210.png"
style="width:6.5in;height:5.26667in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image211.png"
style="width:6.5in;height:3.44653in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image212.png"
style="width:6.5in;height:8.44444in"
alt="A screenshot of a computer Description automatically generated" />

Configure Azure Private Link and Firewall for your Azure Storage Account
instance

- Disable Public Access (you can only access from within your VMs now.)

  - This is done on the network tab as well as the Settings -\>
    configuration “Allow Blob public access → Disabled” as well

  - <img src="./media/media/image213.png"
    style="width:3.78178in;height:2.79206in"
    alt="A screenshot of a computer Description automatically generated" />

Make your settings look like mine. They need to be exact to be able to
satisfy the NIST 800-53 SC-& requirement.

<img src="./media/media/image214.png"
style="width:6.5in;height:5.62639in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image215.png"
style="width:6.5in;height:4.49028in"
alt="A screenshot of a computer Description automatically generated" />

Then, go into private endpoint connections and add a private endpoint.

<img src="./media/media/image216.png"
style="width:6.5in;height:3.98819in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image217.png"
style="width:6.5in;height:3.29306in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image218.png"
style="width:6.5in;height:5.16389in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image219.png"
style="width:6.5in;height:3.29444in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image220.png"
style="width:6.5in;height:7.60486in" />

Observe Network Watcher Topology for the region and resource group all
your stuff.

Observe the Key Vault and Storage Account Private Endpoints are there.

<img src="./media/media/image221.png"
style="width:3.41714in;height:2.31282in"
alt="A screenshot of a computer Description automatically generated" /><img src="./media/media/image222.png"
style="width:6.5in;height:3.64236in"
alt="A screenshot of a computer Description automatically generated" /><img src="./media/media/image223.png"
style="width:3.08376in;height:2.73997in"
alt="A screenshot of a computer screen Description automatically generated" />

Wait 5 minutes (get coffee, etc)

Login to “windows-vm” and check the IP addresses of your Key Vault and
Storage Account instances.

They should be private addresses, indicating the resources have been
probably integrated into private VNet:

<img src="./media/media/image224.png"
style="width:6.5in;height:2.73125in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image225.png"
style="width:6.5in;height:2.64722in"
alt="A computer screen with white text Description automatically generated" />

<img src="./media/media/image226.png"
style="width:6.5in;height:4.69653in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image227.png"
style="width:6.5in;height:2.08472in"
alt="A computer screen with white text Description automatically generated" />

<img src="./media/media/image228.png"
style="width:6.5in;height:4.22313in"
alt="A computer screen shot of a cloud with icons Description automatically generated" />

If you see a public IP address, either it’s not done propagating yet, or
it’s not configured correctly:

<img src="./media/media/image229.png"
style="width:6.5in;height:3.09931in"
alt="A screenshot of a computer Description automatically generated" />

(The above example was done from my personal Mac over the internet, but
this is what it will look like when it’s not working–with the public IP
addresses)

Possible causes for this are your resources and VM are actually in
different Virtual Networks, or something is just not setup right.

The good news is, you don’t need to fix this for the rest of the lab, we
are just trying to lock down the environment. However, if you want to
fix it, you can try deleting the Private Endpoints/config and trying
again.

Now, I will add an NSG like in the diagram above for the subnet itself
since the resources were once available on the public Internet but are
now within its enclosed subnet.

Create it, and no rules need to be set. This is to show it's possible to
attach to a subnet. <img src="./media/media/image230.png"
style="width:6.5in;height:3.38125in"
alt="A screenshot of a computer Description automatically generated" /><img src="./media/media/image231.png"
style="width:4.05265in;height:1.86484in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/media/image232.png"
style="width:6.5in;height:5.21111in"
alt="Screens screenshot of a computer Description automatically generated" />

Now, you can look at the topology on network watchers to see the
connections. Make sure to click on the cross specifically to see the
layout I'm looking at.

<img src="./media/media/image233.png"
style="width:2.18611in;height:1.8375in"
alt="A screenshot of a map Description automatically generated" />

<img src="./media/media/image234.png"
style="width:6.5in;height:5.23472in"
alt="A screenshot of a computer Description automatically generated" />

After this, you can go into Microsoft Defenders for Cloud to check if
the compliance has been updated. There is a Waiting period for it to be
evaluated, so you may not see immediate changes.

**After this lab, we will let our environment run for 24 hours, so
ensure your Windows VM and Linux VM are on. The next day (24 hours
later), we will capture our statistics for the locked-down environment
for our portfolio!**

**After a Secure environment.**

<img src="./media/media/image235.png"
style="width:6.5in;height:2.95903in"
alt="A screenshot of a computer Description automatically generated" />

<span id="_Toc164711437" class="anchor"></span>***<u>Environment
Cleanup</u>***

**<u>Do you need your VMs to be on for this lab?</u>**

**NO**

Delete all resource groups (this will automatically delete all VMs, Data
Collection Rules, Diagnostic Settings, Network Security Groups, Virtual
Networks, etc.)

Go to Azure Active Directory (aka Microsoft Entra ID) and delete the
attacker accounts and any other accounts that are NOT your main account
or breakglass account.

Ensure diagnostic settings have been deleted within Azure Active
Directory (aka Microsoft Entra ID).

Go to Microsoft Defender for Cloud → Environment Settings and
de-provision the subscription and Log Analytics Workspace.

<u>Optional</u>: You can go to Regulatory Compliance in Microsoft
Defender for Cloud and “Delete” the NIST 800-53 policy.

This concludes the lab for making a Virtual honeynet. We have gone over
many policies, concepts, constraints, logging, hardening, analyzing, and
much more. Thanks for reading and following along. Stay updated with my
socials for upcoming news and projects.
