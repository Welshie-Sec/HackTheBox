# CrownJewel-1
## Exec Summary
CrownJewel-1 is the first sherlock I'm doing. It is a DFIR scenario tracking events and files of a compromised domain controller. It's fairly straightforward and doesn't require anything outside of normal Windows usage.
### Stats
* Diffculty: Very Easy
* ~Duration to Complete: Short
* Primary OS Knowledge: Windows
### Tools Used
* Windows Event Viewer
* MFTeCMD (ZimmermanTools)
* Timeline Exlorer (ZimmermanTools)

## Scenario Text
```Forela's domain controller is under attack. The Domain Administrator account is believed to be compromised, and it is suspected that the threat actor dumped the NTDS.dit database on the DC. We just received an alert of vssadmin being used on the DC, since this is not part of the routine schedule we have good reason to believe that the attacker abused this LOLBIN utility to get the Domain environment's crown jewel. Perform some analysis on provided artifacts for a quick triage and if possible kick the attacker as early as possible.```

## Task
### Task 1
```Attackers can abuse the vssadmin utility to create volume shadow snapshots and then extract sensitive files like NTDS.dit to bypass security mechanisms. Identify the time when the Volume Shadow Copy service entered a running state.```

So taking first look at what files were included we are provided with 4 files for this scenario:
* 3 Event logs: Microsoft-Windows-NTFS,SECURITY,SYSTEM
* 1 File called $MFT, which on first assumption is a Master File Table index.

For this first task, we need to identify a timestamp for the the Volume Shadow Copy service to be running. The tool used for this part is Windows Event Viewer.
After importing the event files we then can use the find the command in the Actions bar on the right and look through the SYSTEM to find the event.
[Task1_SYSTEM_Find](https://github.com/user-attachments/assets/f77ca883-2835-4b2f-9a9d-470a62810dae)


The Timestamp can be found by looking at the Details tab and XML view. Keep in mind the Logged time is not the same as time created.

### Task 2
```When a volume shadow snapshot is created, the Volume shadow copy service validates the privileges using the Machine account and enumerates User groups. Find the User groups it enumerates, the Subject Account name, and also identify the Process ID(in decimal) of the Volume shadow copy service process```

For this task we will look into the SECURITY logs, for event codes 4799, and 5379; first code being a security group was enmumerated and the later being that credentials were read.
### Task 3
```Identify the Process ID (in Decimal) of the volume shadow copy service process.```

This one was mentioned in the previous task and can be found in the 4799 event. Answer needs to be converted from hex. I just used the Programmer mode in the Windows calc app.
![Task3_ProcID_ProcName](https://github.com/user-attachments/assets/ed150fd9-9d1d-4768-8a3a-64f44ca6fe90)

### Task 4
```Find the assigned Volume ID/GUID value to the Shadow copy snapshot when it was mounted.```

To find the snapshot that was mounted we would select the Microsoft-Windows-NTFS logs and look at Event ID 4 which are volume mounts.
![Task4_EventCode4](https://github.com/user-attachments/assets/bde060ea-efdf-4746-a4ae-0db53c832205)

### Task 5
```Identify the full path of the dumped NTDS database on disk.```

With this task, we finally leave Event Viewer with all the relevant information needed and utilize it while parsing the $MFT file.
MFTeCMD and Timeline Explorer can be found here a long with many other useful tools. https://ericzimmerman.github.io/#!index.md

First thing we got to do is parse out the $MFT artifact into a CSV file. Just picking this up my export was not great but it created the directory anyways. Important note is that it did ask for admin rights for the command line, so something to keep in mind if you are in a environment where you don't have that access. I used this command on to generate the CSV.

```. .\MFTECmd.exe -f '..\..\Sec\HTB Drops\SHERLOCKS\Artifacts\C\$MFT' --csv ~```

We then import the new CSV into the timeline explorer and apply a filter to to narrow down results to the day of the incident. I used the last modified column and filter as "Is same day". I would have taken a screenshot of the table selections but it would disappear with either print screen or delayed snipping tool so this what you get.

![Task5_LastModified](https://github.com/user-attachments/assets/3a5ea681-9f9c-4fd5-b7f3-4ede3ceb5f47)



We know we are looking for a ntds.dit file so we use a secondary filter in the "File Name" Column, which resulted in only 1 result. The parent path column which should be next to file name should have the answer. For entering the answer, entering C:\.............\ntds.dit was correct.

![Task5_FileLocation](https://github.com/user-attachments/assets/02b099b4-fba5-4961-9776-bfd296c5566e)


### Task 6
```When was newly dumped ntds.dit created on disk?```

This task can be completed by looking at the Created column.

### Task 7
```A registry hive was also dumped alongside the NTDS database. Which registry hive was dumped and what is its file size in bytes?```

This one is pretty straightforward as well. Before removing the file name column filter, double click the Parent Path cell contents for the Task 5 answer and copy it to clipboard. Now clear the File Name column filter and then apply a filter to the Parent Path column by pasting from clipboard. You should have two rows for the result. Grab the hive name from the file name column and the size from the File Size column. This task and Sherlock are now done.
