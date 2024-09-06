# Exec Summary
This is a OSINT Challenge defined as easy on the site...however. Due to the ebb and flow of time, it may not be possible to complete this without requiring other players solutions. The blockchain that some of this information is based on basically no longer exist and it appears that most public archival services for blockchains, no longer have access to the transaction history. This is resolved by using wayback machine, but getting the proper transaction id is problematic now in 2024. I got the flag for this only after yoinking the transaction ID from another writeup so I'm classifying this as being unofficially solved by myself.

* Difficulty: Easy until it's not
* Duration to Complete: Short until it's not.
* Tools used: Google, Waybackmachine

# Challenge Description

```Frank Vitalik is a hustler, can you figure out where the money flows?```

# First Steps
Given the basic challenge description and that's all, we need to probably find something that looks like a flag out on the web, so where do we start?
Lets breakdown what we have so far:
* Someone named Frank Vitalik
* Occupation of Hustler
* Money and it's flow seems to be integral

# Finding Frank
So first we start with a basic google search and ignore running into other writeups (at the time of writing this the challenge is 1575 days old)

https://www.reddit.com/user/frankvitalik/

This user seems to match what we are looking for. References to Crypto scamming and what appears to be a link to a sketchy url.

There is a couple of other sites to check like the X/Twitter postings and a Freelance profile but let's start with Franks free coinz url.
*Post-mortem*
Looking back on this section, the other links were nothings for this, only the reddit user mattered.


# Steemit[.]com
Doing a who is search doesn't lead to much info here, as the domain is a private registration.
https://who.is/whois/steemit.com

Even though this is a challenge and should be relatively safe, I still believe in practicing taking safe steps to collecting information.
So I'm checking the url on hybrid analysis.
https[:]//steemit[.]com/htb/@freecoinz/freecoinz 

Nothing crazy came back, so checking it out on in a browser that has nothing on it seems alright.
What we can see is that the scam is supposedly a double return type of scam with Ethereum coins. It also includes the supposed wallet address.
Checking around, it seems to be a legitamate wallet address, first site didn't show any activity, Etherscan seemed promising but still doesn't have any transactions, makes me wonder how long information like that is kept around.

```0x1b3247Cd0A59ac8B37A922804D150556dB837699```

**Insert freecoinz**

We also technically have another user called freecoinz but I don't consider this a viable route.

It does mention another place called ropsten net, I don't know my cryptocoin history so I'm looking into this.
Ok, turns out ropsten net is the name of a ethereum testnet that basically stopped existing in 2022. Etherscan no longer provides a site for searching this testnet and that's a problem.

So currently hit a brick wall with this, it seems that maybe on first go the only way to get the transaction details for ropsten now is to make a node for it and query the network that may or may not exist still lmao. No one seems to be keeping this historical information pubicly.

Now for the unofficial solution

# Unofficial Solution
Ok so utilizing a previous writeup
```https://sys41x4.arijit-bhowmick.me/blog/HTB/challenge/osint/Money%20Flowz.html```

You can see that they have the flag blocked out. HOWEVER, they did show the transaction ID for the flag which is:

```0xe1320c23f292e52090e423e5cdb7b4b10d3c70a8d1b947dff25ae892609f2ef4```

And luckily enough the wayback machine clutches this up and has a valid capture for the transaction.

```http://web.archive.org/web/20211121151402/https://ropsten.etherscan.io/tx/0xe1320c23f292e52090e423e5cdb7b4b10d3c70a8d1b947dff25ae892609f2ef4```

**Insert picture on wayback machine**

