# PhotoRec-Clarion-File-Recovery-Signatures

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

However because PhotoRec uses a file signature to terminate or end the previous file because its a File Carver [7], in practice only one file signature can be used otherwise PhotoRec would recover a dct file in sections, with the first section recovered file having the 11dct_aa_sig file extension which would be 5 bytes in length with the bytes being: offset for aa + aa aa aa + offset for bb, the second section recovered file having the 11dct_bb_sig file extension and would be 9 bytes in length with the bytes being offset for bb + bb bb + offset for cc, and so on.

So in practice, its best to use just one file signature and hope clarion source code files which have no file signature are not immediately following on from the end of the Dct file on the disk.

From a disaster recovery perspective, its best to locate, the App, Dct and Trf files, becuase the individual source code files and template files have no file signature.

## File System Considerations
Some File Systems are better suited to File Carving data recovery techniques because of the way the data is stored on disk. Whilst techniques like RAID [7] exist for Servers and Desktops, when working in the field, homeless	& rough sleeping, and/or having poor internet connectivity, laptops can benefit from using a File System which encapsulates files with a marker or flag with an indexing system for the situation where a file is broken up across disk sectors. 

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

