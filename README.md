## FATLFN.G4B v0.2 (20241023)

<pre><code>Function: to make Grubutil 'fat' compatible with Long File Names
Available functions: mkfile, mkdir, ren, del, copy, dir. Extra: dirx, dellfn

Use 1: FATLFN.G4B mkdir FILE
Use 2: FATLFN.G4B mkfile size=N FILE
Use 3: FATLFN.G4B copy [/o[:r]] [/a] FILE1 DEVICE2/PATH2/[file2]
Use 4: FATLFN.G4B ren FILE1 [PATH2/]file2
Use 5: FATLFN.G4B del [/r] FILE
Use 6: FATLFN.G4B dir [/a[-]adrsh] [DEVICE]/[PATH[/]]
Use 7: FATLFN.G4B dirx [/a[-]adrsh] [DEVICE]/[PATH[/]]
Use 8: FATLFN.G4B dellfn FILE
Use 9: FATLFN.G4B /? (this text)

Mandatory Arguments:
FILE (contains DEVICE/PATH/NAME.EXT), without DEVICE/ PATH on ROOT: '/'
Function mkfile needs size=N => N is number of bytes, takes Nk/ Nm/ Ng bytes

Optional Arguments:
/o => overwrite
/o:r => overwrite read-only files (extra)
/a => copy attributes (extra)
/r => delete read-only folders/ files (extra)
/a:[-]adrsh => selected attributes [not]only

Remarks:
ren => if PATH1 and PATH2 are equal (on same DEVICE only)
 If Short File Name is renamed to Long File Name, Long File Name is added
  (if none exist)
 If Long File Name is renamed to associated Short File Name, Long File Name is
  removed (use <dirx> before to check)
 If 8+3 Short File Name, not fully highcase (=LFN) is renamed to same name in
  HIGHCASE, 8+3 LFN is removed - and the other way round 8+3 LFN is added
ren => if PATH1 and PATH2 not equal 8+3 Short File Name is moved on same DEVICE
 If source and target share SAME 8+3 Short File Name, Long File Name moved too
    
copy => with /o:r ALL found attributes of read-only file on target got restored
 With /a ALL attributes found on source-file will be set on target-file after
  copying (FAT-source only!)
    
dellfn => if Short File Name is in FILE, Long File Name is searched to delete
 All orphaned Long File Name in directory are deleted too (case-insensitive!)
    
Long File Name with spaces or '=', use double qoutes: "FILE" or '\ ' or '\='
Except for function copy: source FAT-device only

File Versions:
Grub4Dos 0.4.6a, Grubutil FAT >=15/02/2015
 On FAT32 partition >= 4GB use Grubutil FAT from 2023, april or later
Grub4dos for UEFI: compatible, but soon 'Out of malloc memory' errors
Found not compatible with Grub4Dos 0.4.5b / Grub4Dos 0.4.5c
Needed: Loosely Linked Library ATTRIBFT.LLL (>=v0.8.1) - must be in same folder
 as FATLFN.G4B, or already loaded with insmod (NO change of name allowed)
    
Examples:
FATLFN.G4B mkdir (hd0,0)/Directory '' echo %result%
FATLFN.G4B mkdir (hd0,0)/Directory\\ 2 ;; set result
FATLFN.G4B mkfile size=1k (hd0,0)/Directory/Long\ File\ Name
FATLFN.G4B mkfile size=1k "(hd0,0)/Directory/Long File Name 2"
FATLFN.G4B dir (hd0,0)/Directory
FATLFN.G4B dirx (hd0,0)/Directory
FATLFN.G4B ren "(hd0,0)/Directory/Long File Name 2" "Directory/Longer File Name"
FATLFN.G4B ren "(hd0,0)/Directory/Long File Name" /Directory/LONGFI~1
FATLFN.G4B ren "(hd0,0)/Directory/Longer File Name" "Directory 2/LONGER~1"
FATLFN.G4B ren (hd0,0)/Directory/LONGFI~1 /Directory\ 2/Long\ File\ Name
FATLFN.G4B del (hd0,0)/Directory\ 2/Longer\ File\ Name
FATLFN.G4B dellfn "(hd0,0)/Directory 2/Long File Name"
FATLFN.G4B copy (hd0,0)/Directory\ 2/LONGFI~1 (hd0,0)/Directory/somefile</code></pre>

### HISTORY
Version 0.2  
First published version

## FATLFN.LLL v0.2 (20241023)

This is the Loosely Linked Library version of the script.  
The concept of a Loosely Linked Library is an idea of Wonko the Sane (Jaclaz).  
Returns 'result=1' if succes, variable 'message' if failure.  
Exception for functions \<dir> and \<dirx> without "\> nul" at the end: normal operation.  
With "\> nul"  after FILE:  
\<dir> returns 'result=0' if directory is empty, otherwise count of Directory-entries  
\<dirx> returns 'result=0' if directory is empty, otherwise count of Long File Names in Directory  
If a script uses FATLFN.LLL, 'FAT' and 'ATTRIBFT.LLL' must be with insmod loaded before use  
Script must unload 'FAT' and 'ATTRIBFT.LLL' if desired.  
After use of 'FATLFN.LLL' status is always: 'debug 1' AND 'debug msg=0' !  
Use 'FATLFN.LLL help' for help (most of the time identical to command-line version)  

### HISTORY
Version 0.2  
First published version

### SCREENSHOTS

All Examples:  

![FATLFN G4B all examples](https://github.com/user-attachments/assets/995832fa-c03b-4c70-a3d6-6135e47070a5)

