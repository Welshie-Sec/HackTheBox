# Exec Summary
Shattered Tablet is a very easy reversing challenge. It's probably one of the easier challenges to complete and didn't take much time at all. Basically look at the structure of a comparison statement and put a string of characters in the right order for the flag.

# Stats
* Diffculty: Very Easy
* Duration to Complete: Very Short
* Provided Files: 1 binary(tablet)
* Tools: Ghidra, Text editor

# Challenge Description
Deep in an ancient tomb, you've discovered a stone tablet with secret information on the locations of other relics. However, while dodging a poison dart, it slipped from your hands and shattered into hundreds of pieces. Can you reassemble it and read the clues?

# Information Gather

```file tablet```

```tablet: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=71ad3ff9f7e5fbf0edc75446337a0a68deb7ecd6, for GNU/Linux 3.2.0, not stripped```

```strings tablet | less```

Nothing immediately stands out.

# Ghidra

So opening Ghidra, we are presented with the main function and as you can see in the image...it's all just one big if statement.

![Ghidra](https://github.com/user-attachments/assets/7943c50c-259a-487d-a65a-c25755aac92e)

From the looks of everything here, it appears the flag when typed into the input in one go will result in the if statement being true. Given what we know about HTB{flag} format, we can see that the we can just the parts back to together in correct order by treating it like a jigsaw.

# Solution
I grabbed that entire IF statement and put it into a text editor, cleaned it up and started to group the different variables together.

![Solution_Grouped](https://github.com/user-attachments/assets/96d04599-25ac-4eef-a829-bedb7bc73213)

I didn't bother putting them in order until I followed and just put them together in the right order which is what's redacted.
When the string was completed, I tested it against the program and then submitted the correct flag and this one is done!

![AllDone](https://github.com/user-attachments/assets/69f2eb2e-337c-4256-ab95-132f95f6327f)

