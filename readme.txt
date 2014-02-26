Introduction
This a console application that copy files off NTFS volumes by using low level disk reading method. 

Details
The application has 2 mandatory parameters, target file and output path: 
 -param1 is full path to the target file to extract. Also supports IndexNumber instead of file path. 
 -param2 is a valid path to output directory. 


In addition there is an option to also extract all attributes, not just $DATA, by supplying the -AllAttr switch as third parameter. 

This tool will let you copy files that usually is not accessible because the system has locked them. For instance the registry hives like SYSTEM and SAM. Or files inside the "SYSTEM VOLUME INFORMATION". Or any file on the volume. 

It supports input file specified either with full file path, or by its $MFT record number (index number). 

Index number is the best to use, as it will almost certainly garantee a complete file backup. 

Using file path is not fool proof, because the system may have put locks on the parent folder as well. The way the tool currently works with this method is to first test access to the file. If granted, then proceed with extraction. If access not granted, then access the parent directory and resolve its INDX records (its index entries), then when all its "childrens" index numbers are evaluated a file extraction is performed based on filename name match. Of course if the parent directory also is locked, then this method will fail. But then again you would likely not be able to open the directory for browsing anyway. 

So how do you get the index number of a given file. That can be retrived by using MFTRCRD. See its respective wiki page. It will decode a file or directory's $MFT record, including the index entries for directories. That way, although complicated, one can search into directories such as the "SYSTEM VOLUME INFORMATION". Read off the index numbers under the INDX decode, where directories usually are recognized by a size of 0. Re-run MFTRCRD and specify the index number. 


Sample usage

Example for copying the SYSTEM hive off a running system
"RawCopy.exe C:\WINDOWS\system32\config\SYSTEM E:\output" 

Example for extracting the $MFT by specifying its index number
"RawCopy.exe C:0 E:\output" 

Example for extracting MFT reference number 30224 and all attributes including $DATA, and dumping it into C:\tmp:
"RawCopy.exe C:30224 C:\tmp -AllAttr" 
