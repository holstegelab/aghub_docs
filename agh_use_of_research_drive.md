---------------------
# Use of research drive

Due to AGH being a secure compute environment, it is not possible to directly download or upload data from/to the internet.

Instead, we use the SURF Research Drive to transfer data to and from the cluster. Once this has been setup, it is a seamless
experience (copy data into a folder on AGH, and receive it in a folder on your laptop, and vice versa).  Here, we describe how to set this up. 


---------------------
## Step 1. Setting up an account

1. You should have received an invitation mail for the Research Drive (if not, let us know!). Within this mail there is a link to
   create an account. After following this link, there are two options: 
   * If your organization allows logging in through your institutional account, you are advised to use that option. You can then login with  your institutional account credentials.
   * Alternatively, you can select the option to set up a password directly. 

     > **Important:** Note that you will **not** be guided to a new page (**the button *appears* not to work**). 
     > However, instead you will directly receive a (temporary) password in the mail. 

2. After you have created an account, an AGHub Admin has to create and give you access to your own AGHub folder. This can take up to a day during working days. If you have not received permissions after a day (you will receive an email), please contact an AGH Admin.
   Up till then, you will have an empty home folder.

3. After you have received permissions, you can access your AGH research drive folder through the [Research Drive website](https://amsterdamumc.data.surfsara.nl/index.php/login).

----------------------------------------------------
## Step 2. Accessing the Research Drive from your local machine

The research drive can be accessed in multiple ways from your local machine:

* The web browser: [Research Drive](https://amsterdamumc.data.surfsara.nl/).

* A sync client on your local machine. This will automatically sync the contents of the
Research Drive to a folder on your local machine. This way, you can easily transfer data to and from the cluster. 
This client is available for Windows, Mac and Linux. The following page describes how to set up the client:
[ownCloud client](https://servicedesk.surf.nl/wiki/display/WIKI/ownCloud+desktop+client).

* [PREFERRED] RClone. This is a command line tool that allows you to mount the Research Drive to a folder 
on your local machine. When 'mounted', the research folder appears just like a normal folder on your machine. 
This can be accomplished with the tool 'RClone'. Setting up RClone is described on the following page:
[rClone access](https://servicedesk.surf.nl/wiki/display/WIKI/Access+Research+Drive+via+Rclone). Note that the same
rclone config process also has to be performed on the AGH cluster (Step 3). 

-------------------------------------------------
## Step 3. Accessing the Research Drive from the AGH cluster

1. The research drive can be accessed from the cluster through the use of RClone. How to set this up is described on the
   following page: [rClone access](https://servicedesk.surf.nl/wiki/display/WIKI/Access+Research+Drive+via+Rclone).

2. After finishing this, there should be a rclone config being available which the name
   that was chosen during configuration (e.g. 'RD').



----------------------------------------------
## Use of rclone to transfer data to and from AGH

In the previous section, you have set up a configuration with a specific name (e.g. 'RD'). 
Now, `rClone` can be used to transfer data to and from the AGH cluster to the research drive. 
This can be done in two ways:

1. A direct copy command: 

   ```bash
      rclone copy /path/to/local/file RD:your_folder_name/file
	  rclone ls RD:your_folder_name
   ```

   (`your_folder_name` is the name of the folder you see when executing `rclone ls RD:`)


2. [PREFERRED] By mounting the research drive to a folder on the cluster. This can be done by the following command:
   ```bash
       mount_rd RD:your_folder_name
   ```
   This script is provided on AGHub and will excute a rclone mount command (see below), and run this in a so-called 'screen' session in the background.

   Now the `rd` folder in your home folder will contain the data that is on the research drive. Any files you copy in the 
   `rd` folder will appear also automatically on the research drive. And if you have set up the sync client or rclone on your
   own pc, it will also appear automatically on your own pc in the folder that you have set up fo rthis. 
   
>[!NOTE]
> This assumes that you have named your configuration 'RD'. If you have chosen a different name, replace 'RD' with the
> name of your configuration. `your_folder_name` is the name of the folder you see when executing `rclone ls RD:`.


   To unmount the research drive, run the following command:
   ```bash
      unmount_rd
   ```  

>[!NOTE]
> This assumes there is already a folder named `rd` in your home directory, note that this
> folder is automatically created by the init script (see [getting started](agh_getting_started.md)).
> This script will also install the `mount_rd` command. 
> The mounted folder will be empty if you have not copied any files to the research drive yet.

>[!WARNING]
> The mounted folder will only be visible on the user interface machine, and not on the cluster worker nodes. If research drive access is necessary for a cluster job, use the direct `rclone copy` command. 



## Manually excuting rclone mount
   If you prefer, you can also execute the rclone mount command manually.  We advise the following mount options:

   ```bash
       rclone mount --use-cookies --timeout 15m --cache-dir ~/.rd_cache --vfs-cache-mode full --no-modtime RD:your_folder_name  ~/rd
   ```
   - The option '--use-cookies' will make sure that you get connected to the same backend to prevent file locking issues. 
   - The option '--timeout 15m' is needed for the transfer of large files (increase if needed). 
   - The option '--cache-dir .rd_cache --vcf-cache-mode full' will cache files locally to enable random access file access patterns. 
   - The option '--no-modtime' will not update the modification time of files on the research drive (can speed things up).

   You can now move the rclone process to the background by pressing CTRL+Z and then typing 'bg'.
   To unmount the folder, you can move the rclone process to the foreground by typing 'fg' and then pressing CTRL+C.
   Alternatively, you can use the command 'fusermount -u rd' to unmount the folder.


----------------------
## Continue to software installation

Now you are ready for [installation of new software](agh_installing_software.md).



