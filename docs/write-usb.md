# Write USB

The stick will be **wiped**. Confirm the device is a removable USB — never your
internal NVMe/SATA disk.

This page’s Linux path matches an evidence pass that wrote
`ubuntu-24.04.5-live-server-amd64.iso` with `dd`. Sample output uses a ~29 GB
stick at `/dev/sda` labelled `USB DISK 2.0`; your device letter and size will
differ.

## 1. Detect the USB

Plug the stick in, then:

<div class="run" markdown>

```bash
lsblk -o NAME,SIZE,TYPE,FSTYPE,LABEL,MODEL,TRAN,RM
```

```text {.no-copy}
NAME          SIZE TYPE FSTYPE LABEL       MODEL                   TRAN   RM
sda          28.9G disk                    USB DISK 2.0            usb     1
└─sda1       28.9G part vfat   UBUNTU 24_0                                 1
nvme0n1     931.5G disk                    Samsung SSD …           nvme    0
├─nvme0n1p1     1G part vfat                                       nvme    0
└─nvme0n1p2 930.5G part ext4                                       nvme    0
```

</div>

Read the row with `TRAN=usb` and `RM=1`. That whole disk is the target
(example **`/dev/sda`**). The internal OS disk stays on `nvme` / `sata` with
`RM=0` — **do not** use that for `of=`.

Optional: confirm by-id symlink:

<div class="run" markdown>

```bash
ls -l /dev/disk/by-id/usb*
```

```text {.no-copy}
… usb-_USB_DISK_2.0_…-0:0 -> ../../sda
… usb-_USB_DISK_2.0_…-0:0-part1 -> ../../sda1
```

</div>

## 2. Safety gate (recommended)

Refuse to write unless the candidate is still a removable USB disk:

<div class="run" markdown>

```bash
eval "$(lsblk -P -o NAME,SIZE,TYPE,MODEL,TRAN,RM /dev/sda | head -1)"
echo "NAME=$NAME SIZE=$SIZE TYPE=$TYPE MODEL=$MODEL TRAN=$TRAN RM=$RM"
test "$TRAN" = "usb" && test "$RM" = "1" && test "$TYPE" = "disk" \
  && echo "safe_to_flash" || echo "REFUSE"
```

```text {.no-copy}
NAME=sda SIZE=28.9G TYPE=disk MODEL=USB DISK 2.0 TRAN=usb RM=1
safe_to_flash
```

</div>

If you see `REFUSE`, stop and re-check `lsblk`.

## 3. Unmount if needed

<div class="run" markdown>

```bash
sudo umount /dev/sda1 2>/dev/null || true
findmnt /dev/sda /dev/sda1 || echo "not mounted"
```

```text {.no-copy}
not mounted
```

</div>

## 4. Write the ISO with `dd`

Use the **whole disk** (`/dev/sda`), not a partition (`/dev/sda1`). Path assumes
the ISO from [Download ISO](download.md):

<div class="run" markdown>

```bash
ISO="$HOME/Downloads/ubuntu-24.04.5-live-server-amd64.iso"
sudo dd if="$ISO" of=/dev/sda bs=4M status=progress oflag=sync
```

```text {.no-copy}
4076863488 bytes (4.1 GB, 3.8 GiB) copied, 623 s, 6.5 MB/s
972+1 records in
972+1 records out
4080486400 bytes (4.1 GB, 3.8 GiB) copied, 622.851 s, 6.6 MB/s
```

</div>

Then flush caches:

<div class="run" markdown>

```bash
sync
echo synced
```

```text {.no-copy}
synced
```

</div>

!!! tip "Password prompt"
    `dd` to a block device needs root. Run it in a normal terminal so `sudo` can
    prompt. Agent/CI shells without a TTY cannot enter that password for you.

=== "balenaEtcher (optional)"

    If you prefer a GUI: [balenaEtcher](https://etcher.balena.io/) → select the
    ISO → select the USB → Flash. Still confirm you picked the stick, not the
    system drive.

## 5. Confirm the stick looks like an installer

After a successful write, the hybrid ISO layout appears (evidence pass):

<div class="run" markdown>

```bash
lsblk -o NAME,SIZE,TYPE,FSTYPE,LABEL,TRAN,RM /dev/sda
```

```text {.no-copy}
NAME    SIZE TYPE FSTYPE  LABEL                           TRAN RM
sda    28.9G disk iso9660 Ubuntu-Server 24.04.5 LTS amd64 usb   1
├─sda1  3.8G part iso9660 Ubuntu-Server 24.04.5 LTS amd64       1
├─sda2    5M part vfat    ESP                                   1
└─sda3  300K part                                               1
```

</div>

You should **not** still see a single large `vfat` volume labelled `UBUNTU 24_0`
— that was the old contents before `dd`.

Eject before moving the stick to the target machine:

<div class="run" markdown>

```bash
sudo eject /dev/sda || true
echo ready_to_boot
```

```text {.no-copy}
ready_to_boot
```

</div>

## Next

[Install](install.md) — boot the target machine from this USB.
