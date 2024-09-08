# Exec Summary
WIDE is a very easy reversing challenge.

# Stats
* Difficulty: Very Easy
* Time to Complete: Very Short
* File Provided: 1 wide binary, 1 db.ex
* Tools: Ghidra

# Information Gather

So starting this off I'll take a look at some of the basic file info.

```file *```

```
db.ex: Matlab v4 mat-file (little endian) , numeric, rows 1835627088, columns 29557
wide:  ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=13869bb7ce2c22f474b95ba21c9d7e9ff74ecc3f, not stripped
```

```strings wide```

Nothing to interesting here besides a text line on potential usage.

```strings db.ex```

Here we see a line with some interesting things but we will see what that means later.

![strings_db](https://github.com/user-attachments/assets/fd524c0e-664c-4237-b73c-21693326bf23)


# Ghidra

In Ghidra we pull up the main function and have our first look.

![ghidra_main](https://github.com/user-attachments/assets/2ad31034-2722-4c78-a1b4-dd2469467f82)


So looks like it prints some things and interactions with a file. Looking at the usage we see db.ex is the file it will be interacting with.
On line 45, we see a call to another function called menu. Down on line 150, we see a variable with an interesting string. We will save it to a notepad.
Nothing else really stood out after that so we will move on to dynamic interaction with the binary.

![menuFunc_Key](https://github.com/user-attachments/assets/31d386fd-3bf6-48e7-8687-04804ec297d2)


# Solution

```./wide db.exe```

We are met with the flavor text and we can see that a line that we want to interact with like it has large red arrows on it.
Navigating the menu we use numbers 0-6 with 6 being our target. Once selected we are met with a prompt asking for a decryption key. That would be the string we pulled earlier from the menu function.
Paste it in and acquire the flag for this challenge.

![Solution](https://github.com/user-attachments/assets/a52595a4-25c0-42da-ab6e-47f526283450)


