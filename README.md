# Using Google Drive to back up your data By Linux command line.
 
 
## Motivation
It is always important to back up your data, especially if you are a researcher, a scientist, or a data scentist. 
Such disasters can be avoided if you now start to back up your data in different locations. 
As examples, you can use external storage to save your data, or using dropbox, google drive or microsoft Azure in fashion.

Thee prons and crons of those options are really depending on the personalities.
For instance, although the cloud storages is way more expensive, you don't need to worry about anything if you have internet. 
The space of the external storages are much cheaper than the cloud storages, but you can carry them with you. 

However, once either you lose your external storages or the bad section is existed, you lose your back up data. 
In addition, people now attempts to run high resolution numerical simulations in my field (atmosphere/ocean). 
This results in tons of TB/PB dataset is created. As such, that is not making sense to have external disks to save your data unless you have disk array.
 
Up to date, most universities (like umich or tamu) have education cooperation with google, so that the cloud space is nearly unlimited. 
This drives me to an crazy idea that "whether I can backup my data in google drive by command lines."  
If this is possible, then we are able to sync our data to google drive routinely by cron table so that no need to worry about the data lose anymore.

In this document, I will write down how I work on the upload/download/sync my data to google drive.

## Requirement
  - Basic knowledge about scp (actually, you don't need to know it.)
  - How to use linux command line (At least, you need to know how to "goto/copy/move" the directory you want.
  - And most important :  Linux account.
  - gdrive
 
## Installation of gdrive  
#### **All of the examples are based on ada.tamu.edu**
  1. Check what version of your Linux is.
  
  
          login@:~&> uname -a 
        
          Linux login6 2.6.32-696.13.2.el6.x86_64 #1 SMP Thu Oct 5 21:22:16 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux
      
      
     In this case, I know my linux is x86_64 GNU/Linux.
     So that we can go to https://github.com/prasmussen/gdrive seeking for the excuable google drive file in x86_64 version.
     Once we found, we can download it by "wget" command.
     
      
          login@:~&> wget -o gdrive https://docs.google.com/uc?id=0B3X9GlR6EmbnQ0FtZmJJUXEyRTA&export=download
        
        
      When complete the download process, you can find a new file named "gdrive" is donwload.
      If you cannot find it, please looking for uc?id=0B3X9GlR6EmbnQ0FtZmJJUXEyRTA&export=download, and rename it (not necessary).
    
  2. Install gdrive
     To install gdrive, we first need to turn on the permission so that makes the file excuable and then install it. 
     
     
          login@:~&> chmod +x gdrive
        
          login@:~&> sudo install gdrive /user/bin 
        
        
     Notice that, in the most situation, you don't have root permission in the work stations. 
     Therefore, using "sudo" will make your installation failed.
     Instead of, we can ignore the sudo and save it into your local bin directory or the location you usually save bin files.
     
          login@:~&> install gdrive ~/EZSH/.
        
          (In each work station, I always create a directory named "EZSH". 
          This directory save 99% of my excuable files for bash scripts, python, or bin files from other packages.) 
     
     
      If you can find the gdrive is located in your directory, then your installation is succeed. 
      
## link to your school google account.
Now, we are going to link your google account. Please perform the command below.

        login@:~&> gdrive about
        
Following the request, you will complete the link between gdrive command and your google drive.

## upload/download/sync
##### knowledge 1. upload/download is not equal to sync.

  If you are using gdrive to upload/download your file to your google drive, you will obtain all of the files you upload/download.

  For example, There is a file named "cyhsu_is_handsome".
  You now upload it to google drive. After some modification, you upload it again. In this situation, you will see "cyhsu_is_handsome" twice in your google drive.
  In other words, the file is not overwritten, and the file is unable to become "cyhsu_is_double_handsome" or "cyhsu_is_very_handsome".

  On the other hand, if you sync your file to google drive. You will only see one "cyhsu_is_handsome" always.

##### knowledge 2. how to upload/download your file

        login@:~&> gdrive upload "local_file_name"
        login@:~&> gdrive donwload "local_file_name"
      
##### knowledge 3. how to create a new directory

        login@:~&> gdrive mkdir "directory_name"

##### knowledge 4. how to upload/download your file to a specific directory
  In this situation, you want to upload/download file to a specific directory 
  
        login@:~&> gdrive list
        
        Id                                  Name                                  Type   Size       Created
        1vwZzHEgjVpSewKKl579TW0-lWdTcb-WW   test                                  dir               2017-12-02 00:30:52
        1nxMj5fBBRsbgT--gHbfXwXyl9fo1mHHH   TXGLO.ocn.hi.1995-10-02_15:00:00.nc   bin    4.3 GB     2017-12-02 00:29:55
        1Z2ewRepRMvmWRiI5lVSRuS1DHY57-8aa   TXGLO.ocn.hi.1995-10-01_15:00:00.nc   bin    4.3 GB     2017-12-02 00:26:00


  this command option lists all of your files/directories, you need to find the "dic id" for your "specific directory"
  As an example, now you can see there is a directory named "test" which I want to upload my file to there.


        login@:~&> gdrive upload -p  1vwZzHEgjVpSewKKl579TW0-lWdTcb-WW "local_file_name"
        
        
        
##### knowledge 5. how to sync your directory
Since we already have a directory, named "test", I will use "test' directory as an example.


        login@:~&> gdrive sync download --keep-remote 1vwZzHEgjVpSewKKl579TW0-lWdTcb-WW "local_directory_name"
        login@:~&> gdrive sync upload "local_directory_name" 1vwZzHEgjVpSewKKl579TW0-lWdTcb-WW
        login@:~&> gdrive sync list

Now, you should be able to see you have directory named "test" is a syncable directory.
Once You see this, you can sync your direcotry to google drive now by 

        login@:~&> gdrive sync upload "local_directory_name" 1vwZzHEgjVpSewKKl579TW0-lWdTcb-WW

##### knowledge 6. sync upload and sync download
The limitation of gdrive is clearly in upload/download process. When working on upload or download, you can only do one way during processing. 


