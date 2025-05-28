# Sysmon Installation

I began by using Sysmon version 15.15. First, I downloaded the ZIP file containing the Sysmon installation package and extracted its contents onto the target system. Once the files were unzipped and ready, I launched the Command Prompt with administrator privileges to ensure I had the necessary permissions to install and configure Sysmon.

Within the elevated Command Prompt window, I ran the following command:

`Sysmon64.exe -i sysmonconfig-export.xml`

This command installs Sysmon as a system service and instructs it to use the specified configuration file named sysmonconfig-export.xml. This configuration file controls what types of events Sysmon will monitor and log. I didn’t create this configuration file myself; instead, I used a widely respected, community-maintained configuration by SwiftOnSecurity, which is available publicly on GitHub. The configuration file is designed to capture a broad range of important security events and is frequently updated to reflect best practices. You can view or download it here: https://github.com/SwiftOnSecurity/sysmon-config/blob/master/sysmonconfig-export.xml

After running the installation command, I wanted to verify that Sysmon was running properly and logging events as expected. To do this, I opened the Windows Event Viewer application with administrative rights. I then navigated through the event log hierarchy to the following location: Applications and Services Logs > Microsoft > Windows > Sysmon > Operational. This is where Sysmon records its monitored events.

To further confirm the installation and operation of Sysmon, I re-downloaded the Sysmon executable and ran it again on the system. This action generated an event that was successfully logged in the Windows Event Viewer under the Sysmon Operational log. Seeing this entry confirmed that Sysmon was actively monitoring and capturing system activity as configured.

# Splunk Installation

As an Active Directory Domain Controller, Windows Server 2022 is responsible for managing and controlling network resources and user access. It authenticates users when they log in, applies Group Policy Objects (GPOs) to enforce security settings and configurations across the network, and maintains the directory services that organize information about users, computers, and other resources. This role is essential for centralizing authentication and authorization, ensuring that only authorized users and devices can access sensitive data and services within the organization.

### Splunk Server
In addition to its role as a Domain Controller, the same Windows Server also functions as a Splunk Server. Splunk is a powerful platform used for collecting, indexing, and analyzing machine-generated data, such as logs from operating systems, applications, and network devices. By consolidating log data from multiple sources, Splunk helps IT teams and security professionals monitor system health, investigate incidents, and detect anomalies that may indicate security threats.

System Configuration Details
IP Address: The server is assigned a static IP address of 10.0.2.11 with a subnet mask of 255.255.255.0 (/24). Using a static IP ensures consistent communication within the internal network and allows other devices to reliably send data to this server.

Operating System: The server runs on Windows Server 2022, Microsoft's latest server OS version, which offers enhanced security, improved performance, and support for modern hardware and applications.

Installed Software:

Active Directory Domain Services (AD DS): This role service provides directory services and authentication mechanisms.

Splunk Enterprise: The core software for log aggregation, indexing, and analysis.

Detailed Splunk Installation and Configuration Process

1. Install Splunk Enterprise
The first step involves downloading the latest stable version of Splunk Enterprise for Windows from the official Splunk website. After obtaining the installer, the setup wizard is followed to complete the installation. During this process, Splunk is configured to run as a Windows system service, which means it will start automatically when the server boots up, ensuring continuous monitoring without manual intervention. After installation, an admin account is created to securely access the Splunk web interface, where further configuration and monitoring take place.

2. Configure Data Inputs
For Splunk to provide meaningful insights, it needs data from various sources within the network. This is done by setting up Universal Forwarders on other machines—these are lightweight Splunk agents that collect logs and forward them to the central Splunk server. The types of data inputs configured typically include:

Windows Event Logs: Logs from servers and workstations, which contain valuable information on system events, user activities, and security incidents.

Syslog Data: Messages from network infrastructure devices such as routers, switches, and firewalls. These provide visibility into network traffic and device status.

Security Logs from Firewalls and IDS/IPS: Logs generated by security devices help detect and analyze malicious activity or potential breaches.

3. Set Up Dashboards and Alerts
Once data is flowing into Splunk, customized dashboards are created to visualize critical system metrics and security events. Dashboards provide a real-time overview that can highlight anomalies or issues needing attention. In parallel, alerts are configured to notify IT administrators or security teams when specific events occur—for example, repeated failed login attempts, unusual network traffic patterns, or signs of malware activity. This proactive alerting capability is vital for early detection and rapid response to security threats.
