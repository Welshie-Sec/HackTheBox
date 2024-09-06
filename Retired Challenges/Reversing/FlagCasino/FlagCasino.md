# Exec Summary
FlagCasino is a very easy reversing challenge.

# Initial File Info Gather
To start this challenge, I want to collect some information about the provided binary called "Casino"

```file casino                                                                           
casino: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=7618b017ef4299337610a90a0a6ccb7f9efc44a4, for GNU/Linux 3.2.0, not stripped
```

We can see it's a 64bit ELF binary which means it's executable on Linux. Also, a noticeable thing here is that it's symbol tables are not stripped.
Running a strings on it next we don't see an easy flag we can grab but do see some of the structure and how we will most likely interact with the program later on

```strings casino```

**insert picture of Robo ascii that looks like Crow**

# First Look with Ghidra
So, taking a first look at it in Ghidra, we navigate over to the main function and check what's going on.
**Insert Ghidra, main shot**

There isn't anything too crazy, and it looks like the if statement at line 25 is our main focus.

So, in the while loop it will use local_c like a counter and as another thing on line 25. Looking over it, what it's doing on line 25 is being used to define an offset when comparing data from the check data section.
What it's doing is grabbing the offset for check and then comparing that number to the output of RAND which the seed was set by SRAND earlier. This avoids the situation where it would need to display the comparison before which is a solution sometimes when using STRACE for reversing challenges.

For now, I'm going to run the program and see what its normal interactions are.


# Happy Path
Running the program starts us off with some fluff and then waiting for our input. From what we've seen in Ghidra, there is nothing stopping us from putting in any kind of input.
**Insert pic of just random happy path inputs**

Being that this is HackTheBox, it is very common for flags this following format "HTB{s0m3_1337_w0rd1ng}" So when looking for flags we can utilize this as a starting point.
**Insert Prefix guessing**

# SRAND and Rand

From https://cplusplus.com/reference/cstdlib/srand/

We can see that calling SRAND seeds the subsequent calls of rand with its inputted seed which is our inputs when interacting with the program. And also, and more so, this is how the offsets in the if statement are being checked.

Using Ltrace, we can see this playing out. Notice that this lines up with what we are seeing in the check section
**Insert picture of LTRACE***

**Insert picture of check data section**


# Solving
So we know that every time we start the program and put in HTB{ , It will pop out 4 corrects and on a incorrect input it will exit. Now... there are better ways to solve this, but I decided to go with a manual brute force kind of approach... And it worked...
In a bash script I would echo a string and a char in for loop, pipe that to the binary, and grep for the line that says "INCORRECT"; then right after I would echo the specific char for that iteration.
I would then narrow this down by looking for a pattern in the outputs where instead of having an incorrect line and a character, I would have two characters next to each other as soon in this image.
**Insert qr image**
What this tells me through the iteration is that r is the next character as instead of spitting out a incorrect and exiting the program kicked out 5 corrects and just sat there because it should have been waiting for another input that never came. So, I did this until the flag was complete.

Script will follow below but I really recommend finding a better way to capture output.
```bash
#!/bin/bash

CHARS="abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*()_"

for ((i=0; i<${#CHARS}; i++)); do
    echo "HTB{""${CHARS:$i:1}" | ./casino | grep "INCORRECT"
    echo ${CHARS:$i:1}

done
```