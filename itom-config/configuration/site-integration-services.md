# Site Integration Service
Hornbill's Site Integration Service (SIS) is a software package installed on a computer which sits behind an organization's firewall. It runs as a standard Windows service and, once paired with a Hornbill instance, the instance will make the site integration server instance available for servicing the ITOM Job Queue. 

When pairing a Site Integration Server with a Hornbill instance, you are creating a truest relationship between the site integration service and your Hornbill instance. Once a trust relationship is established, there is an implicit bond made between you Hornbill instance and that SIS instance, ensuring that access to the SIS capabilities to execute integrations and automation jobs can only by orchestrated by your instance. 

## How it works
Communications between your instance and the SIS service uses standard secure HTTPS protocol. For security reasons, all connections are established/set up from behind the firewall (the SIS server) to the Hornbill instance (in the cloud).  This means its not possible for any instance other than the authorized Hornbill instance to make use of th SIS service. 

Jobs that are run by the SIS service are queued on your Hornbill instance, the SIS service will retrieve each job to be run and process it as required. There are currently two main job types; Computer Discovery and IT Automation.  Automation jobs are enabled through pre-built packages provided by Hornbill, and, for those customers using the DevOps edition, the packages can be custom-built using the ITOM Package Creator tools.

The SIS server provides a simple web interface, which is accessible from within your network. This simple UI allows for the initial pairing configuration, and once paired, will display of the service's current status.  

## Software Updates
The SIS service is self-updating, it will automatically detect if a new service or package versions are available, and, where required will update the SIS service software, components and packages completely automatically.

## Technical Considerations
### System Requirements
* OS: Windows Server 2012, 2012 R2, 2016, 2019 or 2022
* RAM: 4GB
* Free Disk Space: 10GB
* CPU load is minimal
* Can be run on virtual as well as physical machines.

### Connectivity
* The SIS communicates with a Hornbill instance using the secure HTTPS protocol. All connections are initiated from the SIS service to the cloud service, so there is no need to make any special firewall configurations or open special ports (unless your internal network security policies block normal HTTPS web traffic) in order for the SIS server to function. 
* The pairing between an Hornbill instance and a SIS service is a secure one-to-one binding scheme that makes it impossible for any job execution to happen that is not controlled by the bonded Hornbill instance. 

:::note
Because of the tight security requirements of the connection between a SIS service and a Hornbill cloud instance, there is no support for communications via a proxy services.
:::

### Discovery and Package Deployment
The SIS service is capable of discovering the following devices on your network:

* All Windows computers that are currently supported by Microsoft.
* Unix/Linux/Mac computers that are SSH enabled. 

Individual packages you deploy may have additional OS requirements.

### Firewall Configuration
A Windows firewall rule for Inbound traffic (Local network traffic only) that allows any TCP traffic into the SIS service  is created on installation and named:
* Hornbill SIS Server - Context Callback (TCP - In).

The following outbound ports between the SIS server and the cloud instance are required:
* HTTPS TCP 443

The following ports between the SIS server and Managed Devices are generally used, the exact port(s) used depend on which method is used for communicating with the target computers:
* WinRM - TCP 5985
* DCOM - TCP 135
* DCOM - Range of dynamic ports:
    * TCP 49152-65535 (RPC dynamic ports – Windows Vista, 2008 and above)
    * TCP 1024-65535 (RPC dynamic ports – Windows NT4, Windows 2000, Windows 2003)

Site Integration Service Discovery (Dependant on the discovery mode used)
* Active Directory / LDAP
    * TCP Port 389 (Between the SIS and the AD Domain Controller / LDAP Server)
* Secure Shell (ssh)
    * TCP Port 22 (Between the SIS and target devices)

The discovery process will make use of ICMP (TCP Ping) during the discovery process.

## Toolbar
* **Refresh**<br>A refresh of the list may be required to display any new devices discovered while you are viewing the list.
* **Show**<br>Displays connectors from the selected group.
* **+ Create Group**<br>(Selectable via the drop-down) allows for the Creation of SIS Groups.
* **Paired / Not Paired**<br>Toggle button that allows the display of Paired or Not Paired SIS.
* **Download Site Integration Server**<br>Downloads the on-premise SIS Installer.
* **Move Selected To...**<br>Moves the selected SIS entries to the selected Group.
* **+**<br>Add a new SIS Connector.
* **Delete**<br>Deletes the selected SIS entries.

List
* **Name**<br>The name of the Connector.
* **Group**<br>The group that the Connector belongs to.
* **Description**<br>The user-provided description for the connector.
* **Service Type**<br>Operating system architecture of the Server hosting the SIS installation.
* **Service State**<br>Toggle to enable or disable the SIS.
* **Online Status**<br>The current status of the link to the SIS service.
* **Service Build**<br>SIS Server build version. Any Service showing an older build may highlight that there is an issue with automatic updates for that service.
* **Last Seen On**<br>This will display the last time there was communication between the Hornbill SIS Service and the SIS Server.

## Creating an SIS Service Profile on the Hornbill Instance
1. Navigate to Administration > Hornbill ITOM > Site Integration Services.
1. Click the NewPackageButton.png button to create a new SIS Connector.
1. Enter the following details:
    * Name - name used to identify the SIS server to the Hornbill Instance.
    * Group - Should be a least one default group, others can be selected via drop down if created previously.
1. Click the Create Site Integration Service button.
1. The Authorization Code is displayed, and should be recorded for later use.
1. See the next section for details of how to pair your SIS with your Hornbill instance.
1. Should you choose not to complete the pairing at this time, the connector can be found by selecting the Not Paired filter in your list of SIS Connectors.

:::tip
The Authorization Code is valid for 1 hour. Should the SIS connector and SIS installation not be paired during this time, the key will expire. To generate a new Key, remove the SIS connector and recreate it.
:::

## Downloading and Installing the SIS Service
The Site Integration Service is installed as a Windows Service called "ESPSisService"
The Hornbill SIS is installed as a Windows Service and will require local administration rights for installation on the target computer.

1. Navigate to: Administration > Hornbill ITOM > Site Integration Services
1. Click the Download Site Integration Server button on the toolbar
1. Locate the downloaded executable (.exe), and double click to begin
1. Click Install
1. Click Ok to Confirm the Installation
1. Close the Install dialog
1. Open the Services mmc console
1. Start the EspSisService if it isn't already running

## Pairing an SIS Server with a Hornbill Instance
Once the EspSisService is running, the process of pairing the service with a Hornbill instance can begin, which will require an Authorization Code. This code will have been provided while creating an SIS connector. See the section <a href="#creating-an-sis-service-profile-on-the-hornbill-instance">Creating an SIS Connector on the Hornbill Instance</a>
1. Open a Browser window on the computer where the SIS server is installed, and go to the URL http://localhost:11117, here you will be presented with a form to enter the Instance ID and an Authorization Code needed to pair with your instance. 
1. Enter the Instance ID and Authorization Code.
1. Click the Pair with Instance button.

The Instance ID and valid Authorization Code are required to pair an SIS with your Hornbill instance. Once paired, the SIS status is reviewable via http://localhost:11117.

## Grouping Site Integration Servers
The creation of groups enables SIS connectors and SIS installations to be logically grouped; each SIS entry must belong to a single group. A "Default" group is provided with the option to create additional groups as required. Generally, the network infrastructure, load balancing and failover requirements will determine the number and grouping of SIS installations.

* **Load Balancing**<br>When more than one server is placed within a group, Jobs sent to the group for processing will be processed by the next available SIS server spreading the load.
* **Failover protection**<br>SIS servers poll the Job queue for available jobs, and thus if a server fails, any other server within the same group will pick the next available job. Any job currently being processed by the SIS server will fail, and the status set accordingly. If the job has already been pushed to a client and executed, then it will potentially be orphaned, and the status set to Timed-Out.

::: tip
If a standalone SIS server fails then all jobs aimed at that server will be left in the Job queue and will not be processed until the server is up and running again.
:::

## Creating a Group
1. From the ITOM page select Site Integration Services
1. Click the Show dropdown
1. Click +Create Group option
1. Enter the New Group Name
1. Click Apply

## Removing an SIS Server Installation
If you want to remove the SIS server from your instance. simply navigate to the SIS servers tab in the admin tool and delete the SIS server you want to remove.

The removal of the SIS server software from your server(s) requires manual steps in order to remove the service and all related files.

1. Open Windows Powershell console as an administrator
1. Enter the following:
    * Stop-Service EspSisService
    * sc.exe delete EspSisService
    * Remove-Item "$env:ProgramFiles\Hornbill\Site Integration Server" -Recurse
    * Remove-Item "$env:ProgramData\Hornbill\Site Integration Server" -Recurse
:::tip
Care should be taken with these steps as they will perform a recursive delete on the two folders specified. Once the service is removed don't forget to remove the SIS entry on the instance.
:::
<!-- https://wiki.hornbill.com/index.php?title=Site_Integration_Services -->