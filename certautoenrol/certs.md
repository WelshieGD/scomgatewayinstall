# Auto enrolment
How to enable
- [Autoenrollment](https://docs.cyberark.com/Idaptive/Latest/en/Content/CoreServices/Connector/UserComputerCerts.htm?TocPath=Administrator%7CConfigure%20MFA%7CManage%20AD%20certificates%20in%20devices%7C_____1#:~:text=To%20enable%20the%20Certificate%20enrollment%20policy%20for%20user%20certificates%20expand,Click%20OK)

  - Also ensure Domain Computers can also enrol (security tab)

# SCOM Certificate Template
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
