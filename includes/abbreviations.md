*[2FA]: Two-factor authentication. A second login step after your password, usually a 6-digit code from an app.
*[AGHub]: Amsterdam Genetics Hub. A SURF HPC environment used by the Holstege group and partners.
*[Apptainer]: Open-source container runtime (formerly Singularity) used on HPC systems.
*[Conda]: Package and environment manager used for Python, R, and scientific software.
*[ECDSA]: Elliptic Curve Digital Signature Algorithm. A family of SSH public-key types.
*[EduID]: Personal Dutch academic identity used as a fallback when no institute account is available.
*[HPC]: High-performance computing.
*[QR code]: A square barcode that your authenticator app scans to enrol your account.
*[SLURM]: Job scheduler used to submit and manage batch jobs on the cluster.
*[Spider]: A SURF batch HPC platform; AGHub jobs run on Spider worker nodes.
*[SRAM]: SURF Research Access Management. Controls collaboration membership.
*[SSH]: Secure Shell. The protocol used to log in to AGHub from your computer.
*[Singularity]: Container runtime used on HPC systems (now called Apptainer upstream).
*[SURF]: The Dutch national research IT cooperative that operates AGHub.
*[SURFcua]: SURF Central User Administration. The portal that holds your AGHub login and SSH keys.
*[TOTP]: Time-based one-time password. A 6-digit code that changes every ~30 seconds.
*[WebDAV]: A protocol that lets `rclone` and other clients talk to Research Drive.
*[rclone]: Command-line tool for syncing files between local storage and remote services.

*[authenticator app]: An app that generates the 6-digit TOTP codes used as your AGHub second factor — for example privacyIDEA Authenticator, Google Authenticator, Microsoft Authenticator, or KeePassXC.
*[doornode]: The AGHub login entry host (doornode.hpcv.surf.nl) that brokers SSH connections into AGHub.
*[private key]: The half of an SSH key pair that must stay on your own computer. Never upload or share it.
*[public key]: The half of an SSH key pair (usually a file ending in `.pub`) that you upload to SURFcua to authorise logins.
*[passphrase]: An optional password that protects your local SSH private key file. Not the same as your AGHub/SURFcua portal password.
*[portal password]: The AGHub/SURFcua password you set in Step 2. Used when logging in to AGHub and when changing SSH keys in the portal.
*[Research Drive]: SURF's file-sharing service used to move data in and out of AGHub.
*[WebDAV endpoint]: The URL `rclone` uses to talk to Research Drive, e.g. `https://amsterdamumc.data.surfsara.nl/remote.php/webdav/`.
*[app password]: A separate password generated in Research Drive for tools like `rclone`. Not your normal Research Drive login password.
*[init script]: `/project/aghub/Share/init/init.sh` — the AGHub initialization script you run once on first login.
