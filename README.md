# SCOM Gateway Install

[Documentation](https://learn.microsoft.com/en-us/system-center/scom/deploy-install-gateway-server?view=sc-om-2022&tabs=InstallGatewayServer)

## Summary Steps
### Same domain or kerberos realm as management server
1. Copy GatewayApproval from SCOM support tools folder to setup folder on Management Server e.g. C:\Program Files\Microsoft System Center\Operations Manager\setup

```
"C:\Program Files\Microsoft System Center\Operations Manager\setup\Microsoft.EnterpriseManagement.GatewayApprovalTool.exe" /ManagementServerName=<managementserverFQDN> /GatewayName=<GatewayFQDN> /ManagementServerInitiatesConnection=True /Action=Create
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
- [Autoenrollment](https://docs.cyberark.com/Idaptive/Latest/en/Content/CoreServices/Connector/UserComputerCerts.htm?TocPath=Administrator%7CConfigure%20MFA%7CManage%20AD%20certificates%20in%20devices%7C_____1#:~:text=To%20enable%20the%20Certificate%20enrollment%20policy%20for%20user%20certificates%20expand,Click%20OK)

  - Also ensure Domain Computers can also enrol (security tab)

- [Certreq](https://github.com/WelshieGD/scomgatewayinstall/tree/main/certreq/certreq.md)

- [Audit Collection Services](https://github.com/WelshieGD/scomgatewayinstall/blob/main/acs/certificatemapping.md)

#### SCOM Certificate Template
[Blake Drumms Walk through](https://blakedrumm.com/blog/create-operations-manager-certificate-template/)_

Other considerations.
1. Step 12 certificate request
- Full DN and use ADSIEdit to pull back distinguished name.
- Certificate Subject Name -> Common Name is the FQDN
```
  The specified certificate could not be loaded because the Subject name on the certificate does not match the local computer name. Below is an example of an error.
 Certificate Subject Name : Windows Virtual Machine
 Computer Name            : cust1ms1.langkah.net
```

2. Events:
- 20052 -> Certificate Error
- 20053 -> Certificate Loaded Succesfully

### Daisy Chain Gateways
[Documentation](https://techcommunity.microsoft.com/t5/system-center-blog/how-to-link-multiple-gateway-servers-together/ba-p/341202)
1. The Health Service can only load and use a single certificate. Therefore, the same certificate is used by the parent and child of the gateway in the chain.

2. Run Gateway Approval Tool on Management Server at the end of the chain. 

3. Install Gateway at the other end of the chain and specify the gateway in the middle as the management server
