# CrownJewel-1
## Summary
CrownJewel-1 is the first sherlock I'm doing. So I'm going to be figuring this out as I go <Alan please remove afterwords and put in a correct summary>

## Scenario Text
```Forela's domain controller is under attack. The Domain Administrator account is believed to be compromised, and it is suspected that the threat actor dumped the NTDS.dit database on the DC. We just received an alert of vssadmin being used on the DC, since this is not part of the routine schedule we have good reason to believe that the attacker abused this LOLBIN utility to get the Domain environment's crown jewel. Perform some analysis on provided artifacts for a quick triage and if possible kick the attacker as early as possible.```

## Task
### Task 1
```Attackers can abuse the vssadmin utility to create volume shadow snapshots and then extract sensitive files like NTDS.dit to bypass security mechanisms. Identify the time when the Volume Shadow Copy service entered a running state.```

So taking first look at what files were included we are provided with 4 files for this scenario:
	3 Event logs:
		Microsoft-Windows-NTFS
		SECURITY
		SYSTEM
	1 File called $MFT, which on first assumption is a Master File Table index.

For this first task, we need to identify a timestamp for the the Volume Shadow Copy service to be running. The tool used for this part is Windows Event Viewer.
After importing the event files we then can use the find the command in the Actions bar on the right and look through the SYSTEM to find the event.
**Picture here of find**

The Timestamp can be found by looking at the Details tab and XML view. Keep in mind the Logged time is not the same as time created.

### Task 2
```When a volume shadow snapshot is created, the Volume shadow copy service validates the privileges using the Machine account and enumerates User groups. Find the User groups it enumerates, the Subject Account name, and also identify the Process ID(in decimal) of the Volume shadow copy service process```

For this task we will look into the SECURITY logs, for 
### Task 3
```Identify the Process ID (in Decimal) of the volume shadow copy service process.```
### Task 4
```Find the assigned Volume ID/GUID value to the Shadow copy snapshot when it was mounted.```
### Task 5
```Identify the full path of the dumped NTDS database on disk.```
### Task 6
```When was newly dumped ntds.dit created on disk?```
### Task 7
```A registry hive was also dumped alongside the NTDS database. Which registry hive was dumped and what is its file size in bytes?```