# Use of Research Drive

AGHub has no direct internet access. Use SURF Research Drive to move files between your computer and AGHub.

## What you need

- Research Drive invitation email
- AGHub account already working
- rclone configured on your computer and on AGHub (recommended)

## Step 1: Activate your Research Drive account

1. Open your invitation email and create the account.
2. Sign in with your institutional account if possible, otherwise set a password.
3. Wait until AGHub admins grant access to your personal AGHub folder (can take up to one business day).
4. Verify access on the web portal: [Research Drive login](https://amsterdamumc.data.surfsara.nl/index.php/login)

> **Note:** In password setup flow, the button may appear inactive; in that case you receive a temporary password by email.

## Step 2: Choose how to access Research Drive locally

Options:

- Web browser: [Research Drive](https://amsterdamumc.data.surfsara.nl/)
- Desktop sync client: [ownCloud desktop client](https://servicedesk.surf.nl/wiki/display/WIKI/ownCloud+desktop+client)
- **Recommended:** rclone mount/copy: [Access Research Drive via rclone](https://servicedesk.surf.nl/wiki/display/WIKI/Access+Research+Drive+via+Rclone)

## Step 3: Configure rclone on AGHub

Follow the same SURF rclone instructions on AGHub and create a remote name (examples below use `RD`).

Test:

```bash
rclone ls RD:
```

## Step 4: Transfer files

### Option A (simple): direct copy

```bash
rclone copy /path/to/local/file RD:your_folder_name/file
rclone ls RD:your_folder_name
```

### Option B (recommended): mount Research Drive on AGHub

```bash
mount_rd RD:your_folder_name
```

This mounts Research Drive in `~/rd` and keeps it running in the background.

Unmount:

```bash
unmount_rd
```

> **Note:** `mount_rd` assumes remote name `RD`. If yours is different, replace it.
> **Note:** `~/rd` is created by the AGHub init script in [Getting Started](getting-started.md).
> **Warning:** Mounted Research Drive is visible on the UI/login machine only, not on worker nodes. For jobs, use `rclone copy`.

## Manual mount (optional)

```bash
rclone mount --use-cookies --timeout 15m --cache-dir ~/.rd_cache --vfs-cache-mode full --no-modtime RD:your_folder_name ~/rd
```

## Next step

After file transfer works, continue with [Installation of Software Packages](installing-software.md).
