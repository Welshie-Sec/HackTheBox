# Exec Summary

Regularity is a very easy pwning challenge......


# Initial Information

First thing we look at is the two provided files. One just being the flag file, which indicates that the flag is in a seperate location from the binary we will be interacting with when the instance is spun up and we exploit the vulnerability we will be looking into now.


# Possible RCE

Currently looking into a potential buffer overflow?

 python3 -c 'print("A"*42 +"\x00\x40\x10\x1e" +"\x90"+ "\x63\x61\x74\x20\x66\x6c\x61\x67\x2e\x74\x78\x74"+ "A"*195)' | ./regularity
 ^ This doesn't work, it occurred to me a couple of things, once being that the system I was testing on has full stack/memory protections going on and also I completely forgot the rest of writing buffer overflows. So I will come back to this a bit later.
