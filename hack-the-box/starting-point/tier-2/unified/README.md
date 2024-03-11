## Unified Solution

### Which are the first four open ports?

1. Run the following to discover open ports:
   ```
   nmap -sV  {target_ip)
   ```
   
   ![Screenshot 2024-03-10 at 6 48 51 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/42db2934-de9e-45dc-ad01-ee3a08cdab9e)


**Answer: 22,6789,8080,8443**

## What is the title of the software that is running running on port 8443?

1. We know from the previous section that 8443 is an ssl service. We know that port 8080 is http service. Lets first checkout **{target_ip}:8080** in our browser. This redirects to a website `https://{target_ip}:8443/manage/account/login?redirect=%2Fmanage`
   
![Screenshot 2024-03-10 at 6 54 40 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/424d51b0-791f-49e5-b71d-4a52699dee4e)

2. According to the hint we need to look at the `title`. So we inspect the website in our browser and see that the find that in the `head` tag we find a `title` tag containing **UniFi Network**
   
![Screenshot 2024-03-10 at 6 58 58 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/df1aad6c-3531-4a70-9819-94442093ff1d)

**Answer: UniFi Network** 

## What is the version of the software that is running?

1. Looking at the website we were redirected to we find the version.

   ![Screenshot 2024-03-10 at 7 00 50 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/a32fdb77-3c78-4fb8-b03a-5d61bd3d31f7)

**Answer: 6.4.54**

## What is the CVE for the identified vulnerability?

Hint : Look for UniFi vulnerabilities in 2021

1. Just do a google search for **unifi network version 6.4.54 cve**

**Answer: CVE-2021-44228** 

## What protocol does JNDI leverage in the injection?

1. We can just read the details of CVE that we found [https://nvd.nist.gov/vuln/detail/CVE-2021-44228](https://nvd.nist.gov/vuln/detail/CVE-2021-44228)

**Answer: LDAP ** 

## What tool do we use to intercept the traffic, indicating the attack was successful?

*Hint: Not wireshark*

1. Let's just do a lookup for alternative software to wireshark

**Answer: tcpdump**


## What port do we need to inspect intercepted traffic for?
*Hint: Default port for LDAP*

1. Let's do a lookup for default LDAP port

**Answer: 389**

## What port is the MongoDB service running on?

*Hint: Check the running processes on the system*



