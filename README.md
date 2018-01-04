# Backup GoNow
A backup program that anyone can use the first time, with good defaults!

## Key Features
* Add more files and folders to the backup list using buttons!
* Settings automatically load when the program is started, and save whenever changed.
* Single "launcher" file can be placed anywhere (and generates any files it needs in your user profile).

## License
* Only use and copy in accordance with the LICENSE file.
* Author: Jacob Gustafson ( http://github.com/expertmm http://www.expertmultimedia.com )

## Usage
* After installing the program, modify settings using settings.txt in program's folder.

## Troubleshooting
* If the program fails to load, client needs to download the runtime (such as .NET framework or Mono)

## Changes
(** means requires updating user's script manually if was created with old version)
(*** means requires updating user's main.ini manually if was created with old version)
* (2018-01-03) (in ReconstructedBackupPath workaround Microsoft's Path.Combine bug by removing leading slash from 2nd param of Path.Combine `if (retroactive_string.StartsWith(char.ToString(Path.DirectorySeparatorChar)))`) make retroactive backup go to backup instead of current directory
* (2018-01-03) ** (changed mispelled `__pychache__` to `__pycache__` in default exclusions) prevent backup of __pycache__; and Chrome's "Media Cache"
* (2018-01-03) (fixed by adding `fiDest.MoveTo(fiDest_Retroactive_FullName);`; was non-working except for per-directory operations (as opposed to per-file operations)) retroactive backup folders should contain files
* (2018-01-03) Use previous answer to do retroactive backup (useLastDirectoryDREnable=true by default); added useLastRetroactiveDirectoryAnswer script setting (can be "yes" or "no")
* (2017-04-04) *** During load, remove "loadprofile:" line so doesn't keep getting written over and over to sAllData
* (2017-04-04) after finishing script, try to launch the last run log (silent on exception showing log)
* (2017-04-04) made better description of error and where to find details
* (2017-04-04) ** add many more additions and exclusions to default profile (as generated by writeDefault_BackupScript method)
* (2017-04-04) add ReplacedUserVars method so that environment variables can be used in multiple places
* (2016-02-21) places files in "tl" folder if file path is too long to backup properly otherwise (and records full source path for use during restore) 
* (2016-02-21) single "launcher" file can be placed anywhere and generates any files it needs in your user profile
* (2015-07-30) **, *** Changed default profile -- main.ini: no longer use DestSubFolder option, no longer exclude D:, exclude some standard names of recovery drives (see Developer Notes section below) in order to not use them as destinations; script.txt: add only specific APPDATA and LOCALAPPDATA
* (2012-07-12) Fixed problem where showing incorrect error message due to comparing index >-2 instead of >-1 (added new messages for "being used by another process" & "UnauthorizedAccessException" instead of telling user that the filename was too long in those cases)
* (2012-07-12) only change lByteCountTotalActuallyAdded if Copy succeeded
* (2011-02-22) Moved general file handling and drive management methods to Common.cs and LocInfo.cs, and now these files are references to those from the ForwardFileSync project.
* (2010-12-12) added option to view log (using default txt viewer) under help menu
* (2010-12-12) remove whitespace from beginning and end of line in RunScriptLine method
* (2010-12-12) add RemoveEndsWhiteSpaceByRef method to Common.cs
* (2010-12-12) display line being run (in listbox using the format "   RunScriptLine(...)")
* (2010-11-18) display name of log file when done and say "(statistics)"
* (2010-11-14) Message should say out of space instead of filename too long when out of space exception string contains "system.io.ioexception: there is not enough space on the disk"
* (~2010-11-13) create batch file of failed directory creation and file copy operations 2009-12-10
* (~2010-11-13) send param of Output(...) method (uses lbOut) to Console.WriteLine (done 2009-12-09)
* (~2010-11-13) account for files previously backed up manually (so they aren't deleted).  
* (2010-02-10) estimates time remaining based on last run (using summary.log in profile folder)
* (2009-09-09) shows message when "Removing deleted/moved folder from backup"
* (2009-09-09) no longer overwrites default profile if exists
* (2009-09-08) added profiles feature and default profile (profiles folder must be in current working directory; profile is specified by startup.ini)
* (2009-09-08) Added new setting "AlwaysStayOpen:no" in main.ini: If "no", program closes after run unless there are copy errors, in which case program now asks to review list and "Cancel" quits Backup GoNow.
* (2009-06-09) create reconstructed source path when adding single files with the option to use said path (prevents missing destination folder exception)
* (2009-06-09) delete folders on backup that don't exist on source (previously only files were deleted)
* (2008-10-06) split settings into main.ini and script.txt, so main.ini excludes drives from list before script.txt runs
* (2008-10-06) renamed from GoNowBackup to Backup GoNow
* (2008-10-06) added dependency: FolderLister.cs and Chunker.cs from FolderLister
* (2007-08-15) removed infinite recursion in clean.sh
* (2007-08-15) updated build.sh to account for refactoring
* (2007-08-15) Turned off AutoScale for MainForm
* (2007-07-25) changed default settings to also backup self on linux
* (2007-07-25) updated MainFormFormClosed and MainFormFormClosing to use System.EventArgs
* (2007-07-25) commented broken line in main: Application.SetCompatibleTextRenderingDefault(false);
* (2007-01-25) prevent pushing button twice from crashing program

## Known Issues
* fix retroactive bad backup paths feature uses file date, but is usually earlier than date originall used for unknown reason
* ignore $HOME/.cache by default in BackupGonow (and add this to goldingot profile in .config):
  such as $HOME/.cache/chromium/Default/Media Cache
  Make this work correctly: Exclude:.cache
* (~) note to safely eject drive (or provide option to do that)
* Does not seem to backup any files into "old" folder (for files changed after previous backup), though folders are created (also, avoid issue where path may exceed limit--which may solve overall problem).
* Fix nonsensical read permission errors when copying files from own profile (see end of "C:\Users\Owner\AppData\Roaming\Backup GoNow\1.LastRun Output (read errors when copying).txt")
	such as:
	```
	System.IO.DirectoryNotFoundException: Could not find a part of the path '<Path discovery permission to the specified directory was denied.>'.
	at System.IO.__Error.WinIOError(Int32 errorCode, String maybeFullPath) at System.IO.Directory.InternalCreateDirectory(String fullPath, String path, DirectorySecurity dirSecurity)
	at System.IO.Directory.CreateDirectory(String path, DirectorySecurity directorySecurity)
	at System.IO.Directory.CreateDirectory(String path) at ExpertMultimedia.MainForm.ReconstructPathOnBackup(String sSrcPath)
	in z:\d.cs\Backup GoNow\MainForm.cs:line 861 at ExpertMultimedia.MainForm.BackupFile(String SrcFile_FullName, Boolean bUseReconstructedPath, FileInfo fiNow)
	in z:\d.cs\Backup GoNow\MainForm.cs:line 895
	```
* Fix incorrect datetime format in 1.LastRun Output.txt such as (two dates is ok since first is release date, but 2nd date and date under Output are missing zero padding):
	```
	# Backup GoNow 2015-07-31  2017-5-29 13:47:51
	Output:
	2017-5-29 11:5
	Finished reading C:\Users\Owner\AppData\Roaming\Backup GoNow\profiles\UBERGEMUSE\main.ini (commands processed)
	```
* Fix retroactive backups being placed in unknown location (seemed to have ended up in parent folder of parent folder of working directory (was ownCloud at that time) -- the bug appeared after trying to fix retroactive backup (see "incorrect datetime string.png")
* Should not log exception to console when starting due to trying to save script.txt (via SaveOptions()) while the file is in use
* Prevent copying large files from locking the program with "(Not Responding)" message
* Prevent cyclical backup (a backup source being the backup--account for *NIX systems where drives are under same '/' directory as /home and other directories)
* ? Do not keep retroactive backup of appdata such as firefox by default
* There is no restore option. All restores must be done manually, including files in the tl folder which were too long to backup using the same folder structure.
* Make sure AddFile IGNORES all filters (folder AND file filters!)
* InternalIndexOfPseudoRootWhereFolderStartsWithItsRoot and other drive handling methods should be called with case senstive set to TRUE if sDirSep is '/'.
* Change program menu icon (on menu bar) to custom icon (instead of compiler logo)
* Add separate command in menu to delete all files on dest that aren't on source (should ONLY operate UNDER drive folders, NOT top-level)
* (optimization) do not traverse folder if folder date is same as backup
* always exclude when name with wildcard IsLike: ?:\System Volume Information, ?:\Config.Msi, ?:\RECYCLED, ?:\RECYCLER, ?:\$Recycle.Bin, ?:\.Trashes, ?:\MSOCache, ?:\Boot, ?:\Recovery
	* then change default script to use that method (so that user-created folders by those names aren't excluded)
* IF CAN'T DISPLAY FREE SPACE, show used space instead (count existing space used first)
* Should only say cancelled if user cancelled, instead of finished reading script text file
* fix bug where cannot recreate destination folder (check for DirectoryInfo overlap that would cause inability to write subfolder)
* Now that listbox output is minimal, log file should be written in realtime
  * make sure that EVERYTHING written to listbox is logged
* Make filters AND masks case-insensitive
* Limit size of files to backup (optionally)
* Call conditional refresh (including updating scrollbar values) EVERY time a file begins AND ends copying EVEN IF doesn't need to be copied
	* problem may be caused by copying large files so create thread for file operations.
		* possibly calculate disc write speed (of course, only use files that are actually need to be copied and copy successfully)
			* then create a per-file progress bar for files that take more than a certain amount of time (change to animated bar if reaches expected time then back to regular after done)
			* OR OPTION 2: create a percenage indicator (MUST go up to only 99%!) inside message label and say "exceeded estimated time" if goes over.
* Running Backup GoNow on Windows 7 did NOT remove Firefox's prefs.js from :\Backup\C\Document and Settings\...\
  (did not remove Documents and Settings, possibly because it exists as a virtual folder of some sort in Windows 7)
* Delete folders from backup that are not currently being backed up
  * (low-pri) somehow account for subfolders that were backed up using different masks
  * account for files in folder that will be backed up later in the script
  (see "account for files previously backed up manually" in Changes section)
* add a "Pause" button (multithreaded+callback)

### Needs Confirmation
* (not tested) ! Allow "*" in path (i.e. for Outlook identity or mozilla profile folder)
  * ALREADY IN USE - see default script (firefox profile)
* ! Make backup able to overwrite readonly files that changed (if doesn't)!
* (not tested) Detect if file that is in tl folder (destination path too long) before copying again 
* Fix whatever is causing this Console.Error output (when destination does not seem to be out of space):
	```
	Could not finish backing up file in BackupFile
	System.IO.IOException: There is not enough space on the disk.
		at System.IO.__Error.WinIOError(Int32 errorCode, String maybeFullPath)
		at System.IO.File.InternalCopy(String sourceFileName, String destFileName, Boolean overwrite)
		at OrangejuiceElectronica.MainForm.BackupFile(String sSrcFilePath, Boolean bUseReconstructedPath, FileInfo fiNow)
	Could not finish backing up file in BackupFile
	System.IO.IOException: There is not enough space on the disk.
		at System.IO.__Error.WinIOError(Int32 errorCode, String maybeFullPath)
		at System.IO.File.InternalCopy(String sourceFileName, String destFileName, Boolean overwrite)
		at OrangejuiceElectronica.MainForm.BackupFile(String sSrcFilePath, Boolean bUseReconstructedPath, FileInfo fiNow)
	Process is terminated due to StackOverflowException.
	```

### Low-Priority:
* ? fix commented line in main: Application.SetCompatibleTextRenderingDefault(false); (was commented when changing from .NET 1.1 to 2.0
* Account for situation where the computer is renamed (ask user if they want to rename the profile or start from the default profile)
* Account for DestinationDriveRootDirectory_FullName being allowed to equal to "/" (SetDestFolder behavior that was carried over when SetDestFolder was replaced by IndexChanged) though it doesn't usually end in slash
(currently it is accounted for by always wrapping it like Common.LocalFolderThenSlash(DestinationDriveRootDirectory_FullName) when prepended to a subfolder path

## Developer Notes
* This program requires Microsoft® .NET® framework 2.0 compiler or later, or Mono 2.0 compiler or later to compile.
* Any files in folder named "share" may need to be placed in the working directory of the program
* Requires Common.cs from [ForwardFileSync](https://github.com/expertmm/ForwardFileSync)
* main.ini should always exclude the following standard labels of recovery drives:
	```
	#next line is for Windows 8 (R) default C: drive label on computers such as Lenovo (R) laptop circa 2014
	ExcludeDest:Windows8_OS
	#next line is Lenovo(R) recovery partition
	ExcludeDest:LENOVO
	ExcludeDest:HP_RECOVERY
	ExcludeDest:RECOVERY
	#next line is HP(R) C: drive circa 2006 such as Vista(R)
	ExcludeDest:OS
	#next line is HP(R) factory image circa 2006 such as Vista(R)
	ExcludeDest:FACTORY_IMAGE
	#next line is Dell(R) recovery
	ExcludeDest:Recovery
	ExcludeDest:DELLUTILITY
	```
