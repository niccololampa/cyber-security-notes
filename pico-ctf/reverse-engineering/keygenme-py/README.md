## keygenme-py Solution

This is the solution for [picoCTF's keygenme-py challenge](https://play.picoctf.org/practice/challenge/121?page=2) revese engineeering problem.

![Screenshot 2024-04-06 at 12 00 23 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/e95f0ea9-5fa3-443a-aea2-c14442dd4dbb)

This problem was taken from the picoCTF 2021 and the solution will be discussed below. So proceed with caution.

Let's download the `keygenme-trial.py`

Then after downloading we can quickly view the Python file by running:

```bash
vim keygenme-trial.py
```

Looking at the contents of the file we determine that it is used for some calculation and it appears to be safe.

Let's investigate more by making the file an executable and running it:

```bash
chmod +x keygenme-trial.py
python keygenme-trial.py
```

![Screenshot 2024-04-06 at 12 10 10 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/0bca1750-f8cf-4bca-9a38-16d35323440a)

From experimenting with the program we find that our aim is to unlock the program by entering a license key. 

Looking at the source code `keygenme-trial.py` we find the following variables that are related to the license key. 

```python
key_part_static1_trial = "picoCTF{1n_7h3_|<3y_of_"
key_part_dynamic1_trial = "xxxxxxxx"
key_part_static2_trial = "}"
key_full_template_trial = key_part_static1_trial + key_part_dynamic1_trial + key_part_static2_trial
```
These variables are used in the validation functions executed by menu choice `c`. 

As we can see in the variables above it follows the picoCTF flag format. 

The function we are interested in is `check_key` uses these variables. 

If we  analyze the `check_key(key, username_trial)`, we see that it is first comparing the length of `key_part_static1_trial = "picoCTF{1n_7h3_|<3y_of_"` to the `key` provided by the application's user. 

Once it validates that both variables have have the same length, it then proceeds to checking each character in `key_part_stati1_trial` has the same character in `key` at the same index `i`. This is the static part of the key. 

Next the `check_key` function moves on to the dynamic part. The signifcant code here is `hashlib.sha256(username_trial).hexdigest()`. 

Here the program compares the `key[i]` to specific character index of `hashlib.sha256(username_trial).hexdigest().`

See code below: 

```python
 # Check dynamic part --v
        if key[i] != hashlib.sha256(username_trial).hexdigest()[4]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[5]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[3]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[6]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[2]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[7]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[1]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[8]:
            return False

```


So we need to determine what value hashlib.sha256(username_trial).hexdigest() is equivalent to.

To do this let's copy the original `keygenme-trial.py` to another file in order to modify it. 

`cp keygenme-trial.py keygenme-trial2.py`

Let's insert `hashlib.sha256(username_trial).hexdigest()` in the check_key function before the static key check. 

![Screenshot 2024-04-06 at 1 13 27 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/24d06b87-b8b2-4a96-ac09-d27b31a99cb4)

Running `python keygenme-trial2.py`

We get the string: 
`31250184996c31741ab6ae8452c205deb7dbf431c5bdba21dea5f1289b646bfa`

![Screenshot 2024-04-06 at 1 20 16 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/6791c59f-35e5-43dc-b3b4-221707c087d9)

Now we can print every character equivalent for the dynamic part of the `key`. Insert the following in the `check_key` functhon:
```python
print(hashlib.sha256(username_trial).hexdigest()[4], hashlib.sha256(username_trial).hexdigest()[5], hashlib.sha256(username_trial).hexdigest()[3], hashlib.sha256(username_trial).hexdigest()[6],hashlib.sha256(username_trial).hexdigest()[2],hashlib.sha256(username_trial).hexdigest()[7],hashlib.sha256(username_trial).hexdigest()[1],hashlib.sha256(username_trial).hexdigest()[8])
```
![Screenshot 2024-04-06 at 1 16 35 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/5880862b-5bfe-4e44-9b04-d3f328d24e20)

Running `python keygenme-trial2.py` again. 

![Screenshot 2024-04-06 at 1 21 41 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/97ba8c7f-93f8-427b-bdbe-b49dee567e91)

The program prints `0 1 5 8 2 4 1 9`

We know that the key formula is
```python
key_full_template_trial = key_part_static1_trial + key_part_dynamic1_trial + key_part_static2_trial
```

So basing on what we have derived
```python
key_full_template_trial = "picoCTF{1n_7h3_|<3y_of_" + "01582419" + "}"

key_full_template_trial = "picoCTF{1n_7h3_|<3y_of_01582419}"
```

Now let's run the original `python keygenme-trial.py` and enter the derived key `picoCTF{1n_7h3_|<3y_of_01582419}` to check if we are able to unlock the program.

![Screenshot 2024-04-06 at 1 24 34 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/c94d2a2f-3c88-4989-81e2-41fbafd98757)


**Flag: picoCTF{1n_7h3_|<3y_of_01582419}**

Until next time. Keep learning.

Stay stoked and code. :)

I hope you can voluntarily [Buy Me A Coffee](https://www.buymeacoffee.com/thedatalife) if you found this article useful and give additional support for me to continue sharing more content for the community. :)

**Thank you very much. :)**


