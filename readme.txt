Introduction
This a console application that copy files off NTFS volumes by using low level disk reading method. 

Syntax
RawCopy /ImageFile:FullPath\ImageFilename /ImageNtfsVolume:[1,2...n] /FileNamePath:FullPath\Filename /OutputPath:FullPath /AllAttr:[0|1] /RawDirMode:[0|1|2] /WriteFSInfo:

Explanation of parameters
/ImageFile:
The full path and filename of an image file to extract from. If this param is used, then /ImageNtfsVolume: must be set. Optional.
/ImageNtfsVolume:
The NTFS volume number to extract from. The count only consider NTFS volumes, so any other FS volume will not count. Only used with /ImageFile:.
/FileNamePath:
The full path and filename of file to extract. Can also be in the form of Volume:MftRef. Mandatory.
/OutputPath:
The output path to extract file to. Optional. If omitted, then extract path defaults to program directory.
/AllAttr:
Boolean flag to trigger extraction of all attributes. Optional. Defaults to 0.
/RawDirMode:
An optional directory listing mode. 0 is no print. 1 is detailed print. 2 is basic print. If omitted it defaults to 0. Can be used in conjunction with any of the other parameters, however in order for this it is not possible to define FileNamePath with an MftRef.
/WriteFSInfo:
An optional boolean flag for writing a file with some volume information into VolInfo.txt in the defined output directory.

This tool will let you copy files that usually are not accessible because the system has locked them. For instance the registry hives like SYSTEM and SAM. Or files inside the "System Volume Information". Or pagefile.sys. Or any file on the filesystem.

It supports input file specified either with full file path, or by its $MFT record number (index number). 

So how do you get the index number of a given file that is not one of the known system files? Since version 1.0.0.13 the functionality of RawDir was ported into RawCopy. That way, one can do a search into directories such as the "System Volume Information" (RawCopy.exe /FileNamePath:"c:\System Volume Information" /RawDirMode:2).

For image files the volume letter in the /FileNamePath: parameter is ignored.

The /WriteFSInfo: parameter can be useful when scripting since SectorsPerCluster and MFTRecordSize is used with LogFileParser and Mft2Csv.


Sample usage

Example for copying the pagefile off a running system
RawCopy.exe /FileNamePath:C:\pagefile.sys /OutputPath:E:\output

Example for copying the SYSTEM hive off a running system
RawCopy.exe /FileNamePath:C:\WINDOWS\system32\config\SYSTEM /OutputPath:E:\output

Example for extracting the $MFT by specifying its index number, into to the program directory.
RawCopy.exe /FileNamePath:C:0

Example for extracting MFT reference number 30224 and all attributes including $DATA, and dumping it into C:\tmp:
RawCopy.exe /FileNamePath:C:30224 /OutputPath:C:\tmp /AllAttr:1

Example for accessing a disk image and extracting MftRef ($LogFile) from NTFS volume number 2.
RawCopy.exe /ImageFile:e:\temp\diskimage.dd /ImageNtfsVolume:2 /FileNamePath:c:2 /OutputPath:e:\out

Example for accessing partition/volume image and extracting file.ext and dumping it into E:\out.
RawCopy.exe /ImageFile:e:\temp\partimage.dd /ImageNtfsVolume:1 /FileNamePath:c:\file.ext /OutputPath:e:\out

Example for making a raw dirlisting in detailed mode in c:\$Extend:
RawCopy.exe /FileNamePath:c:\$Extend /RawDirMode:1

Example for making a raw dirlisting in basic mode in c:\System Volume Information inside a disk image file:
RawCopy.exe /ImageFile:e:\temp\diskimage.dd /ImageNtfsVolume:1 /FileNamePath:"c:\System Volume Information" /RawDirMode:2
