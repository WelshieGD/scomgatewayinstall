# SCOM Gateway Install

[Documentation](https://learn.microsoft.com/en-us/system-center/scom/deploy-install-gateway-server?view=sc-om-2022&tabs=InstallGatewayServer)

## Summary Steps
### Same domain or kerberos realm as management server
1. Copy GatewayApproval from SCOM support tools folder to setup folder on Management Server e.g. C:\Program Files\Microsoft System Center\Operations Manager\setup

2. Ensure Gateway appears in the SCOM console

3. Install SCOM Gateway on Gateway Server

4. Ensure SCOM Gateway Server goes healthy

5. Apply cumulative update

6. Configure certificates if going to be connecting to agents \ gateways outside of kerberos realm

### Certificates


