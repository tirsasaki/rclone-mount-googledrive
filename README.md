# Linux-rclone-mount-googledrive-permanent

**Rclone Mount for GoogleDrive,OneDrive Other Permanent**

You can create a systemd service that will automatically mount your Google Drive on startup.

**Steps:**

**Step 1: Create a new systemd service file, for example, rclone-drive.service:**

```sudo nano /etc/systemd/system/rclone-drive.service```

**Step 2: Make a folder GoogleDrive**

```mkdir /home/username/GoogleDrive```

Make sure to replace:

/home/***username***/GoogleDrive

**Step 3: Add the following configuration into the file:**

```[Unit]
Description=Mount Google Drive using rclone
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
ExecStart=/usr/bin/rclone mount remote_name: /home/username/GoogleDrive \
  --config /home/username/.config/rclone/rclone.conf \
  --vfs-cache-mode writes
ExecStop=/bin/fusermount -u /home/username/GoogleDrive
Restart=always
User=username

[Install]
WantedBy=multi-user.target
```

Make sure to replace:

***remote_name***: with the name of your Rclone remote (e.g., drive:).

/home/***username***/GoogleDrive with the path to your mount directory.

/home/***username***/.config/rclone/rclone.conf with the path to your Rclone configuration file (if different).

User=***username***

**Step 4: Reload systemd,enable and start the service:**

```sudo systemctl daemon-reload```

```sudo systemctl enable rclone-drive.service```

```sudo systemctl start rclone-drive.service```

After these steps, Rclone will automatically mount your Google Drive every time the system boots up.


