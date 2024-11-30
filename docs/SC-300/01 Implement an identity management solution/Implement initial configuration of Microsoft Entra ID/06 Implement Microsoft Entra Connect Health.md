- Microsoft Entra Connect Health provides monitoring of your on-premises identity infrastructure
## Requirements

- Microsoft Entra ID Premium is installed.
- You're a global administrator in Microsoft Entra ID.
- The Microsoft Entra Connect Health agent is installed on each targeted server.
- The Azure service endpoints have outbound connectivity.
- Outbound connectivity is based on IP addresses.
- TLS inspection for outbound traffic is filtered or disabled.
- Firewall ports on the server are running the agent.

    - The agent requires the following firewall ports to be open so that it can communicate with the Microsoft Entra Connect Health service endpoints:
        
        - TCP port 443
        - TCP port 5671
    - The latest version of the agent doesn't require port 5671. Upgrade to the latest version so that only port 443 is required.
        
- PowerShell version 4.0 or newer is installed.
- FIPS (Federal Information Processing Standard) is disabled.


## Install the agent

## Install the agent for Active Directory Federation Service
#Note: Your Active Directory Federation Server (AD FS) server should be different from your Sync server. Don't install the AD FS agent on your Sync server.

To verify that the agent was installed, look for the following services on the server. If you completed the configuration, they should already be running. Otherwise, they're stopped until the configuration is complete.

- Microsoft Entra Connect Health AD FS Diagnostics Service
- Microsoft Entra Connect Health AD FS Insights Service
- Microsoft Entra Connect Health AD FS Monitoring Service
## Install the agent for Sync

The Microsoft Entra Connect Health agent for Sync is installed automatically in the latest version of Microsoft Entra Connect.

To verify the agent has been installed, look for the following services on the server. If you completed the configuration, the services should already be running. Otherwise, the services are stopped until the configuration is complete.

- Microsoft Entra Connect Health Sync Insights Service
- Microsoft Entra Connect Health Sync Monitoring Service
