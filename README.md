# Clarion File Recovery Signatures for Forensic Data Tools

## Overview
Using two forensic file recovery programs PhotoRec [1] and OSForensics [2] to recover Clarion App, Dct and Trf file's because they have at least a leading file signature and a trailing file signature which makes it easier to recover using File Carvers, programs which use File Carving [3] to recover files off storage mediums like spin disks.
Source code files like .a, .c, .CLW, .INC, .INT, .TPW and .TPL are pure ascii text files with no file signatures as are most other programming tools and their source code files, which makes it very difficult to recover unless using a raw storage medium scanner that shows the hex in sectors, where it would be possible to recover with human intervention aka known as a manual recovery.  

Chip based storage like (micro)SD card, USB sticks, SSD drives and NVMe drives are a little less reliable due to the volatile nature of memory chips but are still worth attempting a file recovery from. Chip based storage like (micro)SD cards and others employ a technique called wear levelling [4] which needs to be accounted for. If you want to know more about (micro) SD cards, I would highly recommend reading Bunnie:Studio's for a break down on SD cards and also how to spot fake SD cards. I myself have been able to purchase fake USB sticks of Amazon, where they report a larger storage capacity than what they can actually store. For an example of how spin disks can be hacked I would recommend reading SpriteMods.

When formatting a storage medium, there is a debate over whether to use smaller sector sizes or larger sector sizes. Performance of Spin disks is improved with larger sector sizes, chip based storage is best set to match what the storage controller chips reports as best. Provided the storage medium is not routinely filled up and then files deleted, the sector size doesnt really matter, but if using a spin disk to generate code on, a smaller sector size is preferable to aid recovery of source files which have no file signature. This is because smaller files like Clarion source files fit in their own sector with the rest of the sector remaining empty and larger files will span the next aka contiguous sector(s), leaving empty space in the last sector the file occupies. Where it gets harder to recover is when Windows is forced to break up a file and use the spare space in a sector and its at this point that Defragging software will help to keep files spanning as few a sector as possible. Defragging spin disks routinely will always help with file recovery, simply by keeping files spanning sectors contiguous. Likewise purchasing larger hard disks is preferable to reduce the risk from disk failure and the subsequent extra work spent recovering files manually by reading the raw hex on the hard drive, using specialist tools like OSForensics or HxD [7].

Raid [8] , is designed to limit the fallout when a hard drive fails in whole or part. Raid 5 is currently the best, but in a worst case scenerio, the need to understand how Raid methods works makes file recovery a more difficult task, and eliminates simple File Carving tools like PhotoRec, and requires more specialist tools which give access to the raw hard drives. When the hard drive controllers fail, data recovery companies, will remove the Spin/Hard disk drive platter [9] , and slot them into one of their own matching drives where the hard drive controller is known to work, in order to read the data off. Other techniques may also exist today. 

When choosing a Hex Editor to read the contents of a file, HxD [7] is a free and highly recommended Hex Editor and Disk Editor. It doesnt change the hex when copying to other tools like text editors. The addon/plugin Hex Editor [10] for Notepad++ [11] will change/encode the hex when copying and pasting to another text document in NPP, so be warned, if choosing to use it to view files in Hex.

The only other risk with file recovery tools, is whether they write to the drive during the recovery process. Establishing if a tool does this is paramount, which is why for legal reasons and legal reports, its always best to image the storage device first, before attempting any file recovery. The existence of a disk image also makes it possible to load the disk image to cloud servers, where the fastest file analysis and recovery can be attempted, reducing time for intelligence gathering.


[1] https://www.cgsecurity.org/wiki/photoRec

[2] https://www.osforensics.com/

[3] https://en.wikipedia.org/wiki/File_carving

[4] https://en.wikipedia.org/wiki/Wear_leveling

[5] https://www.bunniestudios.com/blog/2012/microsd-card-faq/

[6] https://spritesmods.com/?art=hddhack

[7] https://mh-nexus.de/en/hxd/

[8] https://en.wikipedia.org/wiki/RAID

[9] https://en.wikipedia.org/wiki/Hard_disk_drive_platter

[10] https://github.com/chcg/NPP_HexEdit

[11] https://notepad-plus-plus.org/








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
It has a 16digit character limit.

?9E2080?C5B5??E8

### Trailing 24 bytes

Three Example Clarion 11 Dictionary Files:

            aa aa                                              bb
			
44 7B 61 F2 68 8E DF 67 3C BD 3B FD 86 30 10 97 5E 60 8C 9E 98 26 50 CB

44 7B 61 F2 68 8E DF 67 3C BD 3B FD 86 30 10 97 5E 60 8C 9E 98 26 50 CB

24 2C E2 62 68 8E 1F AE BC FE 0C 1B 8C 10 20 9F 5C 80 EB 0A 88 26 70 CF


aa has an offset of 18 from the file end.

bb has an offset of 2

Photorec does not support trailing signatures, instead relying on space on the hard drive sector(s)/cluster(s) or another file signature to terminate the existing file. 

Valid OSForensics signature optional Footer Pattern starting from the right most character moving left.

????????????26??

?26??


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

686CD6B021594C49


### Trailing 24 bytes

Three Example Clarion 11 App Files:

00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00

00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00

00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00


Photorec does not support trailing signatures.

Valid OSForensics signature Footer Pattern.

0000000000000000


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

66CD6B021594C49B


### Trailing 24 bytes

Two Example Clarion 11 Registry Files:
```

aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa aa

00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00

00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
```


Photorec does not support trailing signatures.

Valid OSForensics signature Footer Pattern.

0000000000000000



### PhotoRec.sig File

The PhotoRec.sig file uses one file signature per line.
More information can be found on their website.

The format is:
[File Extension any reasonable length] [Offset] [0xHex String]

Copy the PhotoRec.Sig file into the folder where the EXE is run from.

The process can take several hours to complete.


### OSForensics Pattern Searchs.

The Leading and optional Trailing Pattern Search can be add manually.
Load the program, select Deleted File Search, click the blue Config... hyperlink right of the Scan button, click the Configure Carving Options button, click the Add button on the bottom left of the popup window, type in the file Extension, eg .App or .Dct or .Trf, then copy the leading search pattern/string into the Header pattern field, and the trailing search pattern/string into the Footer pattern (optional) field. As this is Hex a decimal, the search pattern are not Case Sensitive. 
If you know the spin disk is rather full and has not been defragged or defragged recently, its best to leave the Max File Size as is, if you know the Spin disk is not rather full and/or has been Defragged recently, if you know the maximum file size in bytes for the file extension you are hoping to recover, adjust the Maximum file size accordingly leaving a few extra KB's to help improve the odds of recovery. If you also know an empty file will never be smaller than a certain size in Bytes, also add this. If you dont succeed in recovery the required files, you can experiment with the Base Score on Header match option. Its default is 50%, which is suitable where a trailing search pattern is also specified, but if you dont have a trailing search pattern, consider increasing this to something like 75 to increase the pertinence of the leading search pattern.

The process can take several hours to complete, deselecting other file signatures can speed things up a bit.