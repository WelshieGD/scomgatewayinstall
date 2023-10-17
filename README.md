# SCOM Gateway Install

- [Gateway Install Documentation](https://learn.microsoft.com/en-us/system-center/scom/deploy-install-gateway-server?view=sc-om-2022&tabs=InstallGatewayServer)
- [Authentication and Data encryption](https://learn.microsoft.com/en-us/system-center/scom/plan-security-authentication-data-encryption?view=sc-om-2022)

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
- [Auto enrolment](https://github.com/WelshieGD/scomgatewayinstall/blob/main/certautoenrol/certs.md)

#### Off Domain
- [Certreq](https://github.com/WelshieGD/scomgatewayinstall/tree/main/certreq/certreq.md)

#### Audit Collection Services - Certificate Mapping
- [Audit Collection Services](https://github.com/WelshieGD/scomgatewayinstall/blob/main/acs/certificatemapping.md)

## Events:
- 20052 -> Certificate Error
- 20053 -> Certificate Loaded Succesfully

## Daisy Chain Gateways
[Documentation](https://techcommunity.microsoft.com/t5/system-center-blog/how-to-link-multiple-gateway-servers-together/ba-p/341202)
1. The Health Service can only load and use a single certificate. Therefore, the same certificate is used by the parent and child of the gateway in the chain.

2. Run Gateway Approval Tool on Management Server at the end of the chain. 

3. Install Gateway at the other end of the chain and specify the gateway in the middle as the management server
