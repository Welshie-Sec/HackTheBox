# Exec Summary
Shattered Tablet is a very easy reversing challenge.

# Stats
*Provided Files: 1 binary(tablet)

# Challenge Description
Deep in an ancient tomb, you've discovered a stone tablet with secret information on the locations of other relics. However, while dodging a poison dart, it slipped from your hands and shattered into hundreds of pieces. Can you reassemble it and read the clues?

# Information Gather

```file tablet```

```tablet: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=71ad3ff9f7e5fbf0edc75446337a0a68deb7ecd6, for GNU/Linux 3.2.0, not stripped```

```strings tablet | less```

Nothing immediately stands out.

# Ghidra

So opening Ghidra, we are presented with the main function andas you can see in the image...it's all just one big if statement.

**Insert Ghidra if dragon**

From the looks of everything here, it appears the flag when typed into the input in one go will result in the if statement being true. Given what we know about HTB{flag} format, we can see that the we can just the parts back to together in correct order by treating it like a jigsaw.

# Solution
I grabbed that entire IF statement and put it into a text editor, cleaned it up and started to group the different variables together.

**Group Pic**

I didn't bother putting them in order until I followed and just put them together in the right order which is what's redacted.
When the string was completed, I tested it against the program and then submitted the correct flag and this one is done!

**Pic of tablet verify**