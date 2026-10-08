# Download ISO

Work on the **workstation** (not the target machine). This page uses the
**24.04.5** live-server amd64 image from
[releases.ubuntu.com/24.04](https://releases.ubuntu.com/24.04/) — a newer 24.04
point release is fine if you update the filename.

Official landing page (same files):
[Ubuntu Server download](https://ubuntu.com/download/server).

## 1. Working directory

<div class="run" markdown>

```bash
mkdir -p "$HOME/Downloads"
cd "$HOME/Downloads"
pwd
```

```text {.no-copy}
/home/…/Downloads
```

</div>

## 2. Fetch checksum list and ISO

<div class="run" markdown>

```bash
BASE=https://releases.ubuntu.com/24.04
ISO=ubuntu-24.04.5-live-server-amd64.iso
curl -fL -o SHA256SUMS "$BASE/SHA256SUMS"
curl -fL --continue-at - --progress-bar -o "$ISO" "$BASE/$ISO"
ls -lh "$ISO" SHA256SUMS
```

```text {.no-copy}
-rw-rw-r-- 1 you you  893 … SHA256SUMS
-rw-rw-r-- 1 you you 3.9G … ubuntu-24.04.5-live-server-amd64.iso
```

</div>

`--continue-at -` lets you resume if the transfer drops.

## 3. Verify SHA256

Check **only** the server ISO line (the SUMS file also lists desktop/WSL images
you did not download):

<div class="run" markdown>

```bash
ISO=ubuntu-24.04.5-live-server-amd64.iso
grep " ${ISO}$\| \*${ISO}$" SHA256SUMS | sha256sum -c
```

```text {.no-copy}
ubuntu-24.04.5-live-server-amd64.iso: OK
```

</div>

!!! danger "Do not continue on FAILED"
    If you see `FAILED` or a mismatched hash, delete the ISO and download again.
    Do not write a bad image to USB.

Evidence note: this guide’s authoring pass used
`97f3d7ffb032c3eb3b23d2c8be9cc76e60c2c1f2c0146ba5ba9fe01cafae0fd8` for
`ubuntu-24.04.5-live-server-amd64.iso` (from the published `SHA256SUMS`).

## Next

[Write USB](write-usb.md)
