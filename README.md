## FATLFN.G4B v0.3 (20241112)

<pre><code>Function: to make Grubutil 'fat' compatible with Long File Names
Available functions: mkfile, mkdir, ren, del, copy, dir. Extra: dirx, dira, dellfn

Use 1: FATLFN.G4B mkdir FILE
Use 2: FATLFN.G4B mkfile size=N FILE
Use 3: FATLFN.G4B copy [/o[:r]] [/a] FILE1 DEVICE2/PATH2/[file2]
Use 4: FATLFN.G4B ren FILE1 [PATH2/]file2
Use 5: FATLFN.G4B del [/r] FILE
Use 6: FATLFN.G4B dir [/a[-]adrsh] [DEVICE]/[PATH[/]]
Use 7: FATLFN.G4B dirx|dira [/a[-]adrsh] [DEVICE]/[PATH/][f[ile]]
Use 8: FATLFN.G4B dellfn FILE
Use 9: FATLFN.G4B /? (this text)

Mandatory Arguments:
FILE (contains DEVICE/PATH/NAME.EXT), without DEVICE/ PATH on ROOT: '/'
Function mkfile needs size=N => N is number of bytes, takes Nk/ Nm/ Ng bytes

Optional Arguments:
/o => overwrite target-file
/o:r => overwrite read-only files, original attributes restored (extra)
/a => copy attributes, NT-casebyte included (extra)
/r => delete read-only folders/ files (extra)
/a:[-]adrsh => selected attributes [not]only

Remarks:
ren => if PATH1 and PATH2 are equal (on same DEVICE only)
 If Short File Name is renamed to Long File Name, Long File Name is added (if none exist)
 If Long File Name is renamed to associated Short File Name, Long File Name is removed (use <dirx> before to check)
 If 8+3 Short File Name, not fully highcase (=LFN) is renamed to same name in
  HIGHCASE, 8+3 LFN is removed - and the other way round 8+3 LFN is added
ren => if PATH1 and PATH2 not equal 8+3 Short File Name is moved on same DEVICE
 If source and target share SAME 8+3 Short File Name, Long File Name moved too
    
copy => with /o:r ALL found attributes of read-only file on target got restored
 With /a ALL attributes found on FAT source-file will be set on target-file after copying
 Modification Date and -Time are always copied from FAT source-file

Function 'dir' always whole directory, 'dirx' and 'dira' takes 'file', 'ls'-style
dir => same output as 'fat dir' except Long File Names only (order bit different)
dirx => Short File Names, filesize and associated Long File Names
dira => Short File Names, Attributes, Modification Date and Time,
 Creation Date and Time (WRI) and Last Access Date (ACC) - without filesize

dellfn => if Short File Name is in FILE, Long File Name is searched to delete
 All orphaned Long File Name in directory are deleted too (case-insensitive!)
    
Long File Name with spaces or '=', use double qoutes: "FILE" or '\ ' or '\='
Except for function copy: source FAT-device only

File Versions:
Grub4Dos 0.4.6a >=20170505, Grubutil FAT >=15/02/2015
 On FAT32 partition >= 4GB use Grubutil FAT from 2023, april or later
Grub4dos for UEFI: compatible, but soon 'Out of malloc memory' errors
Needed: Loosely Linked Library ATTRIBFT.LLL (>=v0.9) - must be in same folder
 as FATLFN.G4B, or already loaded with insmod (NO change of name allowed)
    
Examples:
FATLFN.G4B mkdir (hd0,0)/Directory '' echo %result%
FATLFN.G4B mkdir (hd0,0)/Directory\\ 2 ;; set result
FATLFN.G4B mkfile size=1k (hd0,0)/Directory/Long\ File\ Name
FATLFN.G4B mkfile size=1k "(hd0,0)/Directory/Long File Name 2"
FATLFN.G4B dir (hd0,0)/Directory
FATLFN.G4B dirx "(hd0,0)/Directory/Long F"
FATLFN.G4B dira (hd0,0)/Directory/
FATLFN.G4B ren "(hd0,0)/Directory/Long File Name 2" "Directory/Longer File Name"
FATLFN.G4B ren "(hd0,0)/Directory/Long File Name" /Directory/LONGFI~1
FATLFN.G4B ren "(hd0,0)/Directory/Longer File Name" "Directory 2/LONGER~1"
FATLFN.G4B ren (hd0,0)/Directory/LONGFI~1 /Directory\ 2/Long\ File\ Name
FATLFN.G4B del (hd0,0)/Directory\ 2/Longer\ File\ Name
FATLFN.G4B dellfn "(hd0,0)/Directory 2/Long File Name"
FATLFN.G4B copy (hd0,0)/Directory\ 2/LONGFI~1 (hd0,0)/Directory/somefile
FATLFN.G4B copy (hd0,0)/MSDOS.SYS (fd0)
FATLFN.G4B copy /o (hd0,0)/MSDOS.SYS (fd0)
FATLFN.G4B copy /a (hd0,0)/IO.SYS (rd)
FATLFN.G4B dira (rd)/IO
FATLFN.G4B copy /o:r (hd0,0)/IO.SYS (rd)
FATLFN.G4B del /r (rd)/IO.SYS</code></pre>

### ATTRIBFT.LLL
Concept of 'Loosely Linked Library' is an idea of Wonko the Sane (Jaclaz)  
More information and download: https://github.com/deomsh/ATTRIBFT.LLL  

### HISTORY
Version 0.3  
Help: adjustments and new Examples
New: ls-style in functions 'dirx' and 'dira'  
New: function 'dira'  
New: check is grub4dos version is >=20170505  
Bugfix: if 'name' or 'extension' is '!'  
Bugfix: better compatibility is full grub4dos scriptline is in 'path' or 'filename'  

Version 0.2  
First published version

### FATLFN.LLL v0.2 (20241103)

This is the Loosely Linked Library version of command-line script 'FATLFN.G4B'  
The concept of a Loosely Linked Library is an idea of Wonko the Sane (Jaclaz)  
Returns 'result=1' if succes, variable 'message' if failure  
Exception for functions \<dir> and \<dirx> without "\> nul" at the end: normal operation  
With "\> nul"  after FILE:  
\<dir> returns 'result=0' if directory is empty, otherwise count of Directory-entries  
\<dirx> returns 'result=0' if directory is empty, otherwise count of Long File Names in Directory  
If a script uses FATLFN.LLL, 'FAT' and 'ATTRIBFT.LLL' must have been loaded with insmod before use  
Script must unload 'FAT' and 'ATTRIBFT.LLL' if desired  
After use of 'FATLFN.LLL' status is always: 'debug 1' AND 'debug msg=0' !  
Use 'FATLFN.LLL help' for help (most of the time identical to command-line version)  

### HISTORY
Version 0.3  
Function 'dira': with '> nul' on command-line 'result' is count of Short File Names
Further same as FATLFN.G4B   

Version 0.2  
First published version

### SCREENSHOTS

Contains all Examples from the 'help' of command-line script 'FATLFN.G4B':  

![FATLFN G4B all examples](https://github.com/user-attachments/assets/995832fa-c03b-4c70-a3d6-6135e47070a5)

Use of switch '/o', compared with 'fat copy' and extra switches '/o:r', '/a' for copy and '/r' for del. File-attributes, Modification Date/Time, Last Access Date and Creation Date/Time echood as 'Attrib', 'Mod', 'Acc' and 'Wri' with ATTRIBFT.G4B (command-line front-end of ATTRIBFT.LLL):

![FATLFN G4B examples with switches compared with fat copy (with ATTRIBFT G4B attriballecho) v0 2(2)](https://github.com/user-attachments/assets/cf77e5de-f1c4-4c61-a243-41dffca12607)
![FATLFN G4B examples with switches compared with fat copy (with ATTRIBFT G4B attriballecho) v0 2(2) II](https://github.com/user-attachments/assets/dd1c682c-3781-4a1d-832d-4b7d7b5f5e90)

ls-style in function 'dirx' and new function 'dira':  

![FATLFN v0 3 New functionality of dirx and new function dira AND filter file names in ls-style](https://github.com/user-attachments/assets/5aac0c1f-e598-4a07-af27-6ecc686873e1)

### ADDENDUM   
Imported functions from ATTRIBFT.LLL:  
getsfnpath, getsfn, getlfn, makelfn, dellfn  
isreadonly, getattrib, setattrib  
getntcasebyte, setntcasebyte  
getdate, setdate, gettime, settime, getdatetimeattriball  
calclfnhash  

Exported functions by FATLFN.LLL:  
mkfile, mkdir, ren, del, copy, dir.  
dirx, dira, dellfn  
