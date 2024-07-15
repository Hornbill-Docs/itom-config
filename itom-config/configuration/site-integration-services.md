# Site Integration Service
Hornbill's Site Integration Service (SIS) is a Windows NT Server service installed on a server behind an organization's firewall. It runs as a standard Windows NT Service and, once paired with a Hornbill instance, monitors the ITOM Job Queue. The SIS connects to the instance order to retrieve jobs that have been placed on the job queue for execution.

Should jobs be detected during a polling interval, the SIS pulls all information from the paired instance regarding the jobs required to be undertaken on specified devices on the network. There are currently two types of jobs available; Computer Discovery and IT Automation. Automation jobs are powered via pre-built packages provided by Hornbill or can be custom-built using the ITOM Package Creator.

A web interface is provided, which allows for the initial pairing and display of the service's current status. The service is self-updating and will detect if a new version has been made available within the cloud and, if so, download and install it automatically. All communications with the cloud will always be initiated locally from the SIS to the cloud and never from the cloud.

## Technical Considerations
### System Requirements
* OS: Windows Server 2012, 2012 R2, 2016, 2019 or 2022
* RAM: 4GB
* Free Disk Space: 10GB
* CPU load is minimal
* Can be run on virtual as well as physical machines.

### Connectivity
* The SIS communicates with a Hornbill instance using the secure HTTPS protocol.
* Hornbill instances will only respond to a successfully paired SIS.

:::note
Support for communications via a proxy service is not currently available.
:::

### Discovery and Package Deployment
The SIS is capable of discovering the following devices:

* Windows 32/64bit computers that are currently supported by Microsoft
* Unix/Linux/Mac Computers running ssh

Depending on the content of the deployment package, there may be additional OS requirements.

### Firewall Configuration
A Windows firewall rule for Inbound traffic (Local subnet) that allows all TCP traffic into the SIS service executable is created on installation and named:
* Hornbill SIS Server - Context Callback (TCP - In).

The following outbound ports between the SIS server and the cloud instance are required:
* HTTPS TCP 443

The following ports between the SIS server and Managed Devices are required, dependant on which method is adopted to retrieve Inventory details:
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

The following is required to support the TCP Ping test used during discovery (only required if the feature is in use):
* ICMP Echo Reply

## Toolbar
* **Refresh**<br>A refresh of the list may be required to display any new devices discovered while you are viewing the list.
* **Show**<br>Displays connectors from the selected group.
* **+ Create Group**<br>(Selectable via the drop-down) allows for the Creation of SIS Groups.
* **Paired / Not Paired**<br>Toggle button that allows the display of Paired or Not Paired SIS.
* **Download Site Integration Server**<br>Downloads the on-premise SIS Installer.
* **Move Selected To...**<br>Moves the selected SIS entries to the selected Group.
* **+**<br>Add a new SIS Connector.
* **Delete**<br>Deletes the selected SIS entries.


<!-- https://wiki.hornbill.com/index.php?title=Site_Integration_Services -->