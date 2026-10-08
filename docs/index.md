# Overview

Install **Ubuntu Server** on spare x86_64 hardware, enable SSH, and prove you can
log in from another machine on the LAN. That is the whole guide.

Other walkthroughs link here for host prep, then layer their own hostname, user,
packages, and workloads.

## What you will do

1. Download the Ubuntu Server live ISO and verify SHA256
2. Detect the USB stick (`TRAN=usb`), then write the ISO with `dd` (or Etcher)
3. Run the Subiquity installer with the choices this guide dictates
4. Confirm OpenSSH and take the LAN IP
5. Prove SSH from your workstation

## What this does not do

- Desktop GUI install (use Server)
- Cloud-init / cloud images (this path is bare metal USB)
- Project-specific packages (Java, Greengrass, Docker, …) — those stay in the
  consuming lab

## Next

[What you need](prerequisites.md)
