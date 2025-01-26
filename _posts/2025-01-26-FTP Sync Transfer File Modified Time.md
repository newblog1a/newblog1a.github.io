---
published: true
---
filezilla <> Mix
Q: When I upload a file its last modification date is always the upload date.
A: Menu Transfer - Preserve timestamps of transferred files
Please note: For setting timestamps of uploaded files, the FTP server must support the MFMT command.
  https://forum.filezilla-project.org/viewtopic.php?t=35083
  https://forum.filezilla-project.org/viewtopic.php?t=56629
  
freefilesync
The problem is that the FTP server doesn't support the MLSD command that is required to receive accurate file time information.

1. Notice that the file modification times only have minute precision for newer files, and are only precise to the day(!) for older dates. Newer ones also don't show the current year, which creates further ambiguity.

2. To make matters worse, the times are in server-local time zone. Which time zone that is, only the server knows, and it doesn't tell!

Problem 2 can be worked around via the MDTM command. This is what FileZilla is doing. It allows to more or less "guess"timate what the time zone offset is. This cannot be precise because of 1.

But even when 2 is more or less solved, problem 1 still makes such FTP servers more or less unsuitable for a "compare by time and size".

Problem 3: The server doesn't support the MFMT command, which means it's not possible (without more dirty hackery...) to set file modification times. This makes the server almost unsuitable for synchronization and backup (... and therefore almost worthless for any kind of serious work).

The "solution" is to not use old and broken FTP servers. I don't know why such old software is still in use, but apparently it is.

Maybe 80% of FTP servers do in fact return precise time information (MLSD) and also save modification times (MFMT).
  https://freefilesync.org/forum/viewtopic.php?t=8123