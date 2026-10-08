# Troubleshooting

| Symptom | Likely cause | What to try |
| --- | --- | --- |
| No boot menu / USB ignored | Legacy vs UEFI, bad stick | Re-flash USB; force UEFI boot entry |
| Installer hangs on network | No DHCP / dead NIC | Plug Ethernet; or configure Wi‑Fi in Subiquity |
| Wrong disk wiped | Selected USB as target | Restore from backup; always confirm disk name |
| Red `Failed mounting cdrom…` after reboot | Installer USB already removed | Normal — press **Enter**; not an install failure |
| `/` only ~half the disk | Default LVM left free space in `ubuntu-vg` | At Storage, edit **`ubuntu-lv`** size to fill the VG (or grow LV after install) |
| `ssh` inactive after reboot | OpenSSH skipped | `sudo apt-get install -y openssh-server` |
| Connection timed out | Wrong IP / firewall / Wi‑Fi AP isolation | Re-check `ip a`; same LAN/VLAN as workstation |
| Permission denied | Wrong user/password, or password auth off | Use the Subiquity username; confirm **Allow password authentication** was checked |

## Next

[References](references.md)
