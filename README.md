# SCOM Gateway Install

[Documentation](https://learn.microsoft.com/en-us/system-center/scom/deploy-install-gateway-server?view=sc-om-2022&tabs=InstallGatewayServer)

## Summary Steps
### Same domain or kerberos realm as management server
1. Copy GatewayApproval from SCOM support tools folder to setup folder on Management Server e.g. C:\Program Files\Microsoft System Center\Operations Manager\setup

```
"C:\Program Files\Microsoft System Center\Operations Manager\Server\Microsoft.EnterpriseManagement.GatewayApprovalTool.exe" /ManagementServerName=<managementserverFQDN> /GatewayName=<GatewayFQDN> /ManagementServerInitiatesConnection=True /Action=Create
```

3. Ensure Gateway appears in the SCOM console

4. Install SCOM Gateway on Gateway Server

```
%WinDir%\System32\msiexec.exe /i path\Directory\MOMGateway.msi /qn /l*v C:\Logs\GatewayInstall.log
ADDLOCAL=MOMGateway
MANAGEMENT_GROUP="<ManagementGroupName>"
IS_ROOT_HEALTH_SERVER=0
ROOT_MANAGEMENT_SERVER_AD=<ParentMSFQDN>
ROOT_MANAGEMENT_SERVER_DNS=<ParentMSFQDN>
ACTIONS_USE_COMPUTER_ACCOUNT=0
ACTIONSDOMAIN=<DomainName>
ACTIONSUSER=<ActionAccountName>
ACTIONSPASSWORD=<Password>
ROOT_MANAGEMENT_SERVER_PORT=5723
[INSTALLDIR=<path\Directory>]
```

5. Ensure SCOM Gateway Server goes healthy

6. Apply cumulative update

7. Configure certificates if going to be connecting to agents \ gateways outside of kerberos realm

### Certificates
#### Active Directory Enrollment
[Documents](https://docs.cyberark.com/Idaptive/Latest/en/Content/CoreServices/Connector/UserComputerCerts.htm?TocPath=Administrator%7CConfigure%20MFA%7CManage%20AD%20certificates%20in%20devices%7C_____1#:~:text=To%20enable%20the%20Certificate%20enrollment%20policy%20for%20user%20certificates%20expand,Click%20OK)

Also ensure Domain Computers can also enrol (security tab)

#### SCOM Certificate Template
[Blake Drumms Walk through](https://blakedrumm.com/blog/create-operations-manager-certificate-template/)_

Other considerations.
1. Step 12 certificate request
- Full DN and use ADSIEdit to pull back distinguished name.
- Certificate Subject Name must match DN
