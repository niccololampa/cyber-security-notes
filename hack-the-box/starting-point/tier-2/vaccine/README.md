## Vaccine Solution

### Besides SSH and HTTP, what other service is hosted on this box?
1. Run an nmap scan with the `-sV` (version of services) `-sC`(default scripts for service discovery and vulnerability detection) 
   ```bash
   nmap -sV -sC {target_machine_ip}
   nmap -sC -sV 10.129.251.21
   ```
![Screenshot 2024-02-19 at 8 50 14 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/6e42d6b3-b7cf-4f42-978f-8213b6bb9f10)

**Answer: ftp**

### This service can be configured to allow login with any password for specific username. What is that username? 

As with the nmap results above the `sC` option was able to determine that **Anonymous FTP login allowed**. If you do a google search the username for this is usually `anonymous`

**anonymous**

## What is the name of the file downloaded over this service?

1. Let's connect via ftp

```bash
ftp {target_machine_ip}
```

2. For username just use `anonymous` and just enter blank password.
3. Enter `ls` and you will find `backup.zip`
4. Run `get backup.zip`

![Screenshot 2024-02-19 at 11 41 56 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/5a467605-0e2d-44f8-b10c-26c3235f9790)



**Answer: backup.zip**

### What script comes with the John The Ripper toolset and generates a hash from a password protected zip archive in a format to allow for cracking attempts?

1. We try to unzip the backup.zip however a password is required.
   ![Screenshot 2024-02-20 at 12 00 57 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/ae3c46ff-0939-4956-8cc9-8d8c92440424)


2. Do a quick google search and you will find the answer

**Answer: zip2john** 


**What is the password for the admin user on the website?**

1. Let's run the `zip2john` script to crack the password. First let's obtain the password hashes.
   ```bash
   zip2john backup.zip > zip.txt
   ```
 
   ![Screenshot 2024-02-20 at 11 08 05 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/1a4ac24e-c684-4583-8f3d-77fd3d0e2db3)

2. Then let's crack the password for the backup.zip
   ```bash
   john zip.txt   
   ```
   ![Screenshot 2024-02-20 at 11 08 56 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/ed883cff-3296-4b69-b6cb-88d6a3d2633a)


3. Now let's unzip backup.zip with the password `741852963`. This gives us two files `index.php` and `style.css`
   ```bash
   unzip backup.zip
   ```
![Screenshot 2024-02-20 at 11 10 22 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/d214ebb6-65ae-4578-ab97-ad4f922a922b)

4. Now let's investigate the index.php `vim index.php`. Here we find a hashed password `2cb42f8734ea607eefed3b70af13bbd3` for the username `admin`. 
   
![Screenshot 2024-02-20 at 11 21 43 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/89307436-b4f4-4097-8c85-98c40531cd06)

5. Let's analyze the hash type by going on any hash analyzer. We use [https://hashes.com/en/tools/hash_identifier](https://hashes.com/en/tools/hash_identifier) in this case and find that it is MD5 format.

   ![Screenshot 2024-02-20 at 11 35 10 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/f49117bb-77a6-4b85-beab-f66fc95b04db)


6. Create a file called `admin-hash.txt` then insert the hash found in the previous step. Let's crack the hash password using john
```bash
john --wordlist=/usr/share/wordlists/rockyou.txt --format=raw-md5 admin-hash.txt
```
![Screenshot 2024-02-20 at 11 42 25 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/39740423-3616-4cf0-a75f-c1af601dd2e7)


**Answer: qwerty789**





