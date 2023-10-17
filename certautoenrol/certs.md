##SCOM Certificate Template
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
