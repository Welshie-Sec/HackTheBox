# Exec Summary
CrownJewel-2 is a follow up to another Sherlock by the same name.

# Stats
* Difficulty: Very Easy
* Duration to Complete: Short
* Tools used: Windows Event Viewer
* Files Provided: 3 evtx files (APPLICATION, SECURITY, SYSTEM)


# Scenario Description
```Forela's Domain environment is pure chaos. Just got another alert from the Domain controller of NTDS.dit database being exfiltrated. Just one day prior you responded to an alert on the same domain controller where an attacker dumped NTDS.dit via vssadmin utility. However, you managed to delete the dumped files kick the attacker out of the DC, and restore a clean snapshot. Now they again managed to access DC with a domain admin account with their persistent access in the environment. This time they are abusing ntdsutil to dump the database. Help Forela in these chaotic times!!```

# Task

## Task 1
```When utilizing ntdsutil.exe to dump NTDS on disk, it simultaneously employs the Microsoft Shadow Copy Service. What is the most recent timestamp at which this service entered the running state, signifying the possible initiation of the NTDS dumping process?```

For this task we start off looking for when Shadow Copy started. I found it by using the find command and just searching for instances starting. The source of the log is different from the first CrownJewel needed to change the approach a little bit but the method worked well enough. remember to take the timestamp from details. Task 1-5 results can be found in the APPLICATION Logs.

![Task1_ShadowStart](https://github.com/user-attachments/assets/ec0269fc-494d-428c-9ea2-c88a174cdb30)


## Task 2
```Identify the full path of the dumped NTDS file.```

Going up from this starting point, we eventually come upon an event showing a non-standard directory. This one was pretty straight forward as part of the path does just say dump_tmp so, really not much to explain on this one.

![Task2_dumpPath](https://github.com/user-attachments/assets/92408b64-0cf2-48f2-b4cb-47af78603d7a)


## Task 3
```When was the database dump created on the disk?```

This task is just a follow-up for the same event from Task 2

## Task 4
```When was the newly dumped database considered complete and ready for use?```

What we are looking for here is when the database engine stopped its instance. From what I understand this means that the copying process completed at this point and ended its task.

![Task4_InstanceComplete](https://github.com/user-attachments/assets/31cd5373-51d3-4843-8a15-e155d5b7a3c2)

## Task 5
```Event logs use event sources to track events coming from different sources. Which event source provides database status data like creation and detachment?```

Pretty straightforward here, for the task answer. It can be found in the Source column. If for some reason you don't see it in the log list frame, you can right click the bar and add it as a column.


## Task 6
```When ntdsutil.exe is used to dump the database, it enumerates certain user groups to validate the privileges of the account being used. Which two groups are enumerated by the ntdsutil.exe process? Also, find the Logon ID so we can easily track the malicious session in our hunt.```

Task 6 slightly differs from a similar task in the previous sherlock, while looking at event codes 4799 we will see similar parts of the answer in which groups were enumerated but instead of specifically a user for the third part, we want the LOGIN ID for the user used.

![Task6_LoginID](https://github.com/user-attachments/assets/83c9ee8b-4045-4f9e-a0aa-3fb7160218bd)



## Task 7
```Now you are tasked to find the Login Time for the malicious Session. Using the Logon ID, find the Time when the user logon session started.```

In the SECURITY logs, I searched using the find action with the Logon ID and word sessions separately and found a 4698 event that shows the correct Logon ID and that a scheduled task was created.
The name of the task reads out as 

![Task7_ScheduledTask](https://github.com/user-attachments/assets/90e45a22-b616-46fc-a75d-7d0e3a79767d)

```\CreateExplorerShellUnelevatedTask```

Which is interesting and plays into the scenario for both CrownJewels. Being a triggered backdoor, it looks like it started up after the domain controller came back online meaning that this was used to maintain persistence from the original attack.
