.. _agh_getting_started:

*******************
AGH Getting Started
*******************


---------------
Quick Overview
---------------

The Alzheimer Genetics Hub (AGH) is a secure compute environment based largely from a clone of the Spider cluster at SURF.

-----------------------------------
Join the Collaborative Organization
-----------------------------------

Accept invitation e-mail from SRAM. By clicking the button, you get a welcome message. Next, click login.

You have then two options:

1. [PREFERRED] Login through your instutional account on which you received your invitation. To search the correct link,
enter the name of your institute in the text box. Many institutes have already been linked. If yours is not among them, 
let us know!


After logging in, it is possible that you receive a message that your institute has not activated access to 
SURF Research Access Management (SRAM). In that case, you need to contact your ICT helpdesk. Store the information in the 
error message in a screen shot, and mention that you want to request that the IdP service of your institute is 
activated/linked to SRAM.


2. As a fallback option, there is the option to use the EduID service. This can be used also with a non-institutional mail account.
To find the EduID service, enter 'EduID' in the text box. You will then see a list of EduID services. Select the one that
is appropriate for you. If you do not have an EduID account yet, you can now create one. This is a one-time process. 
You also will have to download an EduID app (offered by Surf) to enable 2-factor authentication. Next, you can use your EduID account
to login. 



After logging it, it `can take 15 minutes for the system to sync`. 
If you still cannot login after 15 minutes, please contact us.

------------------------
Set-up AGH User Account
------------------------

1. Next, go to [portal](https://portal.cua.surf.nl) and change the auto-generated password from the e-mail and accept the SURF Usage agreement
2. Now, set up a [2fa token](https://2fa.surfsara.nl/) for logging into SSH. 


---------------------------------
2-Step Login (doornode --> aghub)
---------------------------------


First step log-into the doornode with password set in the SURF portal
`ssh sram-aghub-[First initial][Last name]@doornode.surfsara.nl`

After logging into the doornode, select aghub which should appear in a single select list

Re-enter the password from the portal
Enter the OTP you registered earlier

Success if logging in succeeded you should see the AGHub banner

----------------------
Initalize your account
----------------------

To facilitate setting up your account, we recommend you run the following command:

.. code-block:: bash

    /project/aghub/Share/init/init.sh
    

This script will do the following:

* It will copy a conda environment to your home folder, and add it to your path. This conda environment contains 
many useful packages, both general software and software for dealing with genetics data.

* It will also create a folder in your home directory `rd` which will be used to mount the
research drive. 

* It will create a bin folder in your home directory, to which it will add some useful scripts

* It will setup your bashrc, vimrc and screenrc files with useful defaults. 


The next step now is to setup your research drive mount: :ref:`_agh_research_drive`.

Afterwards, you can find out how to install new software: :ref:`_agh_installing_software`.

----------------------
Get started with SLURM
----------------------

After getting access to the cluster, please refer to our Spider documentation for submitting your first jobs:
`Spider Documentation <https://wiki.surfnet.nl/display/SRAM/Invite+admins+and+members+to+a+collaboration/>`_ 










