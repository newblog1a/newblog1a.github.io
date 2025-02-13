---
published: true
---
LOCATION, LOCATION, LOCATION
----------------------------

The key location you need to know about is shown below, %USERNAME% is your current Windows username. We'll call this the working directory.

C:\Users\%USERNAME%\AppData\Local\Google\Chrome\User Data\Default\IndexedDB

NOTE: The "Default" towards the end is the name of the Chrome profile you are working with. If you have multiple profiles you'll have multiple paths like this with profile names replacing the "Default". I don't use multiple profiles and I can't remember ever seeing anyone use them so chances are "Default" is all you need to know about.

BACKING UP THE DATA
-------------------

Within the working directory are two sub-folders called:

chrome-extension_eggkanocgddhmamlbiijnphhppkpkmkl_0.indexeddb.blob
chrome-extension_eggkanocgddhmamlbiijnphhppkpkmkl_0.indexeddb.leveldb

These are the folders that contain the current working data for Tabs Outliner and they need to be backed up. I use the free [Veeam Backup Community Edition](https://www.veeam.com/virtual-machine-backup-solution-free.html) to backup my whole home folder and store it on my NAS, this captures the above folder as part of that process.

You can also expand the whole tree in the Tabs Outliner window and right click Save As on that window. This will give you a web page that contains all the tabs in your tree. This feature really only seems to be useful for emergencies though.

RESTORING THE DATA
------------------

This process is, unfortunately, a bit involved but relatively simple.

1.  Uninstall Tabs Outliner if it's currently installed. Note that you have to uninstall it, disabling it is not enough. Tabs Outliner stores internal backups of the data. If you don't uninstall it before restoring it'll over-write your restored data with an internal backup.
2.  Open explorer and navigate to the working directory then copy in the two folders mentioned in the "Backing Up the Data" section.
3.  Reinstall Tabs Outliner from the extensions store. When you open the Tabs Outliner overview window it will read the data in the two folders you just restored and re-populate your tab tree.

ref
https://www.naturalborncoder.com/2021/06/tabs-outliner-backup-and-restore/