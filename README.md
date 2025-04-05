# Clarion File Recovery Signatures for Forensic Data Tools

## Overview
Using two forensic file recovery programs PhotoRec [1] and OSForensics [2] to recover Clarion App, Dct and Trf file's because they have at least a leading file signature and sometimes a trailing file signature which makes it easier to recover using File Carvers, programs which use File Carving [3] to recover files off storage mediums like spin disks.
Source code files, CLW, INC, INT, TPW and TPL are pure ascii text files with no file signatures, which make it very to recover unless using a raw storage medium scanner that shows the hex in sectors, where it would be possible to recover with human intervention aka known as a manual recovery.  

Chip based storage like (micro)SD card, USB sticks, SSD drives and NVMe drives are a little less reliable due to the volatile nature of memory chips but are still worth attempting a file recovery from. Chip based storage like (micro)SD cards and others employ a technique called wear levelling [4] which needs to be accounted for. If you want to know more about (micro) SD cards, I would highly recommend reading Bunnie:Studio's for a break down on SD cards and also how to spot fake SD cards. For an example of how spin disks can be hacked I would recommened reading SpriteMods. 


[1] https://www.cgsecurity.org/wiki/photoRec

[2] https://www.osforensics.com/

[3] https://en.wikipedia.org/wiki/File_carving

[4] https://en.wikipedia.org/wiki/Wear_leveling

[5] https://www.bunniestudios.com/blog/2012/microsd-card-faq/

[6] https://spritesmods.com/?art=hddhack


## Clarion 11 Dictionary File Signatures

### Leading 24 bytes

Three Example Clarion 11 Dictionary Files: 
  
   aa aa aa    bb bb       cc cc       dd dd dd    ee ee    ff ff ff
   
88 9E 20 80 AB C5 B5 76 90 E8 19 0C 3D 9D 50 E9 AD 0E B5 46 B6 DB CF A6

80 9E 20 80 AF C5 B5 D4 8C E8 19 0C 7F 9D 50 E9 EF 0E B5 48 B6 DB CF BE

8C 9E 20 80 AB C5 B5 E6 8C E8 19 8C 3F 9D 50 E9 A7 0E B5 48 B6 DB CF 86


aa has an offset of 1 from the file start.

bb has an offset of 5

cc has an offset of 9

dd has an offset of 14

ee has an offset of 17

ff has an offset of 20


Valid PhotoRec signatures for the 'photorec.sig' file, but only use one signature in the 'photorec.sig' file.
 
11dct_aa_sig 1 0x9E2080	
	
11dct_bb_sig 5 0xC5B5
		
11dct_cc_sig 9 0xE819
		
11dct_dd_sig 14 0x9D50E9
	
11dct_ee_sig 17 0x0EB5
		
11dct_ff_sig 20 0xB6DBCF


Valid OSForensics signature Header Pattern. OSForensics allows the use ? as a wild card, enabling a more accurate file signature.

?9E2080?C5B5??E819??9D50E9?0EB5?xB6DBCF

### Trailing 24 bytes

Three Example Clarion 11 Dictionary Files:

            aa aa                                              bb
			
44 7B 61 F2 68 8E DF 67 3C BD 3B FD 86 30 10 97 5E 60 8C 9E 98 26 50 CB

44 7B 61 F2 68 8E DF 67 3C BD 3B FD 86 30 10 97 5E 60 8C 9E 98 26 50 CB

24 2C E2 62 68 8E 1F AE BC FE 0C 1B 8C 10 20 9F 5C 80 EB 0A 88 26 70 CF


aa has an offset of 18 from the file end.

bb has an offset of 2

Photorec does not support trailing signatures, instead relying on space on the hard drive sector(s) or another file signature to terminate the existing file.

Valid OSForensics signature optional Footer Pattern.

688E???????????????26??


## Clarion 11 Application File Signatures 

### Leading 24 bytes

Three Example Clarion 11 App Files:

aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa
 
68 6C D6 B0 21 59 4C 49 BA B7 E9 C4 B1 35 A3 35 DE 66 C5 A1 B1 3D 37 4D
 
68 6C D6 B0 21 59 4C 49 BA B7 E9 C4 B1 35 A3 35 3D 4C 1C D5 63 80 44 4B
 
68 6C D6 B0 21 59 4C 49 BA B7 E9 C4 B1 35 A3 35 00 00 00 00 00 00 00 00
 

aa has an offset of 0 from the file start.

Valid PhotoRec signature for the 'photorec.sig' file.

11app_aa_sig 1 0x686CD6B021594C49BAB7E9C4B135A335


Valid OSForensics signature Header Pattern.

686CD6B021594C49BAB7E9C4B135A335????????


### Trailing 24 bytes

Three Example Clarion 11 App Files:

00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00

00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00

00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00


Photorec does not support trailing signatures.

Valid OSForensics signature Footer Pattern.

000000000000000000000000000000000000000000000000


## Clarion 11 Registry File Signatures 

### Leading 24 bytes

Two Example Clarion 11 Registry Files:

aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa

68 6C D6 B0 21 59 4C 49 BA B7 E9 C4 B1 35 A3 35 02 24 F0 DF 2A 5E 94 43

68 6C D6 B0 21 59 4C 49 BA B7 E9 C4 B1 35 A3 35 02 24 F0 DF 2A 5E 94 43


aa has an offset of 0 from the file start.

Valid PhotoRec signature for the 'photorec.sig' file.

11trf_aa_sig 1 0x66CD6B021594C49BAB7E9C4B135A3350224F0DF2A5E9443


Valid OSForensics signature Header Pattern.

66CD6B021594C49BAB7E9C4B135A3350224F0DF2A5E9443


### Trailing 24 bytes

Two Example Clarion 11 Registry Files:

aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa

00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00

00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00


Photorec does not support trailing signatures.

Valid OSForensics signature Footer Pattern.

000000000000000000000000000000000000000000000000
























Clarion File Recovery Signatures for use with CGSecurity.org [1] Photorec [2] File Recovery.

Photorec finalizes a file once it finds another starting File Signature, so whilst you can unselect all file types to speed up the file recovery, in practice, its best to leave all file types selected and include the clarion file signatures in the custome file signature file called 'photorec.sig'.

## photorec.sig folder locations
Copy 'photorec.sig' to the folder where the exe is located, or the '%UserProfile%', or the '%HomePath%' folders.
Paste '%UserProfile%' or '%HomePath%' into the File Explorer Address Bar.

## Working out the Clarion file signatures 
To obtain the file signature, use a hex editor like HxD Hex Editor [3].
The Notepad++ Hex Editor [4] addon will encode the bytes when copying and pasting, eg convert 00 to 20 but HxD Hex Editor does not.

To obtain the file signature, its best to analyse a minimum of 3 files to establish the hex file signature.
Open the Clarion file in question and copy the bytes into a file called photorec.sig in the format below. 

For more information visit : https://www.cgsecurity.org/wiki/Add_your_own_extension_to_PhotoRec

Format:
File Extension - 2 digit Clarion Version with leading zero, 3 char File Type Extension.
Offset of the signature.
First few bytes of the file, also known as the File Signature or Magic Number [5,6].

Three Example Clarion 11 Dictionary Files:
  
   aa aa aa    bb bb       cc cc       dd dd dd    ee ee    ff ff ff
88 9E 20 80 AB C5 B5 76 90 E8 19 0C 3D 9D 50 E9 AD 0E B5 46 B6 DB CF A6
80 9E 20 80 AF C5 B5 D4 8C E8 19 0C 7F 9D 50 E9 EF 0E B5 48 B6 DB CF BE
8C 9E 20 80 AB C5 B5 E6 8C E8 19 8C 3F 9D 50 E9 A7 0E B5 48 B6 DB CF 86

PhotoRec can not handle wildcards, so using the example above to build a file signature that uses aa, bb, cc, dd, ee and ff is not possible.
aa has an offset of 1
bb has an offset of 5
cc has an offset of 9
dd has an offset of 14
ee has an offset of 17
ff has an offset of 20

So these would all be valid Clarion 11 Dct File Signatures:
11dct_aa_sig 1 0x9E2080		
11dct_bb_sig 5 0xC5B5		
11dct_cc_sig 9 0xE819		
11dct_dd_sig 14 0x9D50E9	
11dct_ee_sig 17 0x0EB5		
11dct_ff_sig 20 0xB6DBCF

However because PhotoRec uses a file signature to terminate or end the previous recognised file because its a File Carver [7]. 

In practice only one file signature can be used otherwise PhotoRec would recover a dct file in sections, with the first section recovered file having the 11dct_aa_sig file extension which would be 5 bytes in length with the bytes being: offset for aa + aa aa aa + offset for bb, the second section recovered file having the 11dct_bb_sig file extension and would be 9 bytes in length with the bytes being offset for bb + bb bb + offset for cc, and so on. 

Because Text files like Clarion source and Template files have no file signature, if they follow a file with a file signature, they can be included as part of the recognised file with a file signature.

The Clarion Registry and Clarion App files use the same signature, so if a recovered App or TRF file exists, try the file with its alternative file extension.

## File System Considerations
Some File Systems are better suited to File Carving data recovery techniques because of the way the data is stored on disk. Whilst techniques like RAID [7] exist for Servers and Desktops which can make data recovery much easier without any machine downtime, when working in the field, homeless & rough sleeping in the UK, and/or having poor internet connectivity in remote parts of the world, single disk Windows laptops and USB drives can benefit from using a file system which is predisposed for data recovery.
One such file system which is still in beta as more features are ported across is the OpenZFSonWindows [10] file system. The file system lets you create a copy of the files, so  

## File Carving adaptions
File Carving is also a technique which can be built upon and used to scan Ram, in order to locate a running app where it can then be altered at runtime. In theory the technique can still be used despite Address Space Layout Randomisation (ASLR) [8] mitigations introduced in Linux in 2003 and Microsoft Windows in 2007 [9]. 



Example PhotoRec.sig custom file signatures.

06app 0 0x0000000000020034010000340100744F7053
06dct 0 0x000000000002004A0000004A0000744F7053
11trf 0 0x686CD6B021594C49BAB7E9C4B135A3350224
11app 0 0x686CD6B021594C49BAB7E9C4B135A335
11dct 1 0x9E2080



[1] https://www.cgsecurity.org/
[2] https://en.wikipedia.org/wiki/PhotoRec
[3] https://mh-nexus.de/en/hxd/ 
[4] https://sourceforge.net/projects/npp-plugins/
[5] https://en.wikipedia.org/wiki/File_format#Magic_number
[6] https://en.wikipedia.org/wiki/List_of_file_signatures
[7] https://en.wikipedia.org/wiki/File_carving
[ ] https://en.wikipedia.org/wiki/RAID
[8] https://en.wikipedia.org/wiki/Address_space_layout_randomization
[9] https://learn.microsoft.com/en-us/windows/security/operating-system-security/device-management/override-mitigation-options-for-app-related-security-policies
[10] https://github.com/openzfsonwindows


https://www.reddit.com/r/datarecovery/wiki/software/

https://www.diskgenius.com/how-to/fix-raw-partition-without-losing-data.php#recover_data_from_RAW_partition

https://www.osforensics.com/download.html