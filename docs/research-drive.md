# Use of Research Drive

AGHub has no direct internet access. Use SURF Research Drive to move data
between your computer and AGHub.

The recommended workflow is:

1. Activate your Research Drive account.
2. Make sure your AGHub project folder is visible in the Research Drive web portal.
3. Create Research Drive WebDAV credentials.
4. Configure `rclone` on your computer and on AGHub.
5. Use `rclone copy` for jobs and large transfers, or mount Research Drive for interactive work.

## What You Need

- a Research Drive invitation email
- a working AGHub account
- access to the Research Drive web portal
- WebDAV credentials created in Research Drive

??? info "Install rclone on your local computer"

    Install `rclone` from the official [rclone downloads](https://rclone.org/downloads/)
    or [rclone installation instructions](https://rclone.org/install/). Package
    manager versions can be old, so the upstream installer is usually safer.

## Step 1: Activate Your Research Drive Account

1. Open the Research Drive invitation email and follow the link to create your account.
2. Use institutional login if your organization supports it. Otherwise, choose the password setup option.
3. If the password setup button appears not to respond, check your email. In some flows a temporary password is sent directly by email instead of opening a new page.
4. Wait until an AGHub administrator grants access to your personal or project AGHub folder. This can take up to one business day.
5. Sign in to the [Amsterdam UMC Research Drive portal](https://amsterdamumc.data.surfsara.nl/index.php/login) and check that your AGHub folder is visible.

!!! note "Empty file area on first login"

    Until the AGHub folder permission has been added, the Research Drive web
    portal may show an empty or incomplete file area.

## Step 2: Choose Local Access

You can access Research Drive from your own computer in several ways. For
AGHub, `rclone` is recommended because it works well for command-line
transfer and large datasets.

??? info "Other Research Drive access options"

    - **Web browser:** use the [Amsterdam UMC Research Drive portal](https://amsterdamumc.data.surfsara.nl/).
    - **Desktop sync client:** use the [Nextcloud desktop client](https://servicedesk.surf.nl/wiki/spaces/WIKI/pages/117178931/RD+Getting+the+Nextcloud+desktop+app) for file synchronization.
    - **Recommended for AGHub transfers:** use `rclone`, configured through WebDAV.

    For large datasets, command-line transfer with `rclone` is usually more
    reliable than browser upload. For many small files, consider creating an
    archive first, then transferring the archive.

## Step 3: Create WebDAV Credentials

Create WebDAV app credentials before configuring `rclone`.

1. Open the [Amsterdam UMC Research Drive portal](https://amsterdamumc.data.surfsara.nl/).
2. Click your profile or user icon in the upper-right corner and open **Settings**.
3. In the left menu, open **Security** and find the app password or WebDAV credentials section.
4. Enter an app name such as `rclone-local` or `rclone-aghub`, then click **Create new app password**.
5. Copy the generated username and password to a password manager. Leave the window open until you have copied both — the password is shown only once.
6. Return to the Files page and open **Files settings** in the lower-left corner.
7. Copy the WebDAV endpoint URL. For the Amsterdam UMC instance it is normally:

    ```text
    https://amsterdamumc.data.surfsara.nl/remote.php/webdav/
    ```

??? info "Why WebDAV credentials are separate from your login"

    `rclone` does not use your normal browser session. It needs separate
    WebDAV app credentials from Research Drive.

    Create separate WebDAV app passwords for different uses when possible,
    for example one for your laptop and one for AGHub. If one device is
    lost or a token needs to be replaced, you can revoke only that app
    password.

SURF's detailed reference is
[RD: How to get your WebDAV credentials](https://servicedesk.surf.nl/wiki/spaces/WIKI/pages/117179045/RD+How+to+get+your+WEBDAV+credentials).

## Step 4: Configure Rclone

Run the same configuration on your local computer and on AGHub. The examples
use the remote name `RD`; if you choose another name, replace `RD` in all
commands.

Start the interactive configuration:

```bash
rclone config
```

Answer the prompts:

1. Choose `n` for **New remote** and name it `RD`.
2. Select the storage type **WebDAV**.
3. Enter the WebDAV endpoint URL from Research Drive, for example `https://amsterdamumc.data.surfsara.nl/remote.php/webdav/`.
4. Select the WebDAV vendor **Nextcloud**.
5. Enter the WebDAV username generated in Research Drive.
6. Choose `y` when asked whether you want to type your own password, then paste the generated WebDAV password (twice if asked to confirm).
7. Confirm the configuration.

??? info "What the rclone configuration looks like"

    The resulting `rclone.conf` entry should look similar to this:

    ```ini
    [RD]
    type = webdav
    url = https://amsterdamumc.data.surfsara.nl/remote.php/webdav/
    vendor = nextcloud
    user = <webdav-username>
    pass = <encrypted-password>
    ```

Test the remote:

```bash
rclone lsd RD:
rclone ls RD:
```

The `lsd` command lists directories. Use it to find the exact AGHub folder
name to use in later commands.

SURF's detailed rclone reference is
[RD: How to use Rclone with Research Drive](https://servicedesk.surf.nl/wiki/spaces/WIKI/pages/117179081/RD+How+to+use+Rclone+with+Research+Drive).

## Step 5: Transfer Files With Rclone

### List files

```bash
rclone ls RD:
rclone lsd RD:
```

If a path contains spaces, put the remote path in quotes:

```bash
rclone ls "RD:My project with spaces"
```

### Copy data

Copy a local file or directory to Research Drive:

```bash
rclone copy /path/to/local/file RD:your_folder_name/
rclone copy /path/to/local/folder RD:your_folder_name/folder
```

Copy data from Research Drive to AGHub:

```bash
rclone copy RD:your_folder_name/input ~/input
```

`rclone copy` transfers new or changed files and does not delete files from
the destination.

??? warning "Sync and check commands"

    Use `sync` only when you want the destination to become identical to
    the source. It can delete files from the destination.

    Always test first:

    ```bash
    rclone sync --dry-run /path/to/local/folder RD:your_folder_name/folder
    ```

    If the dry run is correct:

    ```bash
    rclone sync /path/to/local/folder RD:your_folder_name/folder
    ```

    Check whether source and destination match:

    ```bash
    rclone check /path/to/local/folder RD:your_folder_name/folder
    ```

## Step 6: Use Rclone on AGHub

After configuring `RD` on AGHub, you can either copy files directly or mount
Research Drive for interactive use.

### Recommended for jobs: direct copy

Use direct `rclone copy` in batch workflows. Mounted Research Drive is
available on the UI/login machine, but not reliably inside worker-node jobs.

```bash
rclone copy RD:your_folder_name/input ~/input
rclone copy ~/results RD:your_folder_name/results
```

### Convenient for interactive work: mount

The AGHub initialization script creates `~/rd` and installs helper commands.

Mount a folder interactively:

```bash
mount_rd RD:your_folder_name
```

This mounts the Research Drive folder at `~/rd` and keeps the mount process
in the background.

Unmount:

```bash
unmount_rd
```

!!! note "`mount_rd` assumes the remote is named `RD`"

    If your remote has a different name, use that name instead of `RD`.

!!! warning "Mounts and SLURM jobs do not mix"

    Mounted Research Drive is visible on the AGHub UI/login machine. For
    SLURM jobs, stage data with `rclone copy` before the job starts and
    copy results back after the job finishes.

??? info "Manual mount command and options"

    If you prefer to run the mount yourself:

    ```bash
    rclone mount --use-cookies --timeout 15m --cache-dir ~/.rd_cache --vfs-cache-mode full --no-modtime RD:your_folder_name ~/rd
    ```

    Recommended options:

    - `--use-cookies` keeps the session on the same Research Drive backend, which helps avoid file locking issues.
    - `--timeout 15m` gives large file transfers more time. Increase it for very large files.
    - `--cache-dir ~/.rd_cache` and `--vfs-cache-mode full` improve behavior for software that expects random file access.
    - `--no-modtime` avoids updating modification times on Research Drive and can speed up some operations.

    To move a foreground mount to the background, press `Ctrl+Z` and then run `bg`.

    To unmount manually:

    ```bash
    fusermount -u ~/rd
    ```

??? info "Large files and verbose troubleshooting"

    For large uploads, set a longer timeout. A practical rule is about 10
    minutes per GB of the largest file in the transfer. For example, for a
    5 GB file:

    ```bash
    rclone copy --use-cookies --timeout 50m ~/my_5gb_file.bin RD:your_folder_name/
    ```

    Use verbose mode when troubleshooting:

    ```bash
    rclone -v copy ~/data RD:your_folder_name/data
    rclone -vv copy ~/data RD:your_folder_name/data
    ```

## Troubleshooting

| Problem | Fix |
| --- | --- |
| `rclone` asks for a password repeatedly | Check that you used the WebDAV app password, not your normal Research Drive login password. |
| The WebDAV password is lost | Create a new app password in Research Drive. Existing app passwords cannot be shown again. |
| `rclone ls RD:` works locally but not on AGHub | Configure `rclone` separately on AGHub; the local laptop configuration is not automatically copied. |
| A mounted folder appears empty | Confirm the remote path with `rclone lsd RD:` and check that your AGHub folder permissions have been granted. |
| Uploads of large files fail | Add `--use-cookies` and increase `--timeout`. |

## Next Step

After file transfer works, continue with
[Installation of Software Packages](installing-software.md).
