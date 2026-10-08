# Agent notes

Zensical walkthrough: **Ubuntu Server from ISO to SSH-ready host**. Generic on
purpose — consumers (Greengrass, SeisComP, other labs) set hostname, username,
and LAN details in their own docs.

## Commands

```bash
python3 -m venv .venv
.venv/bin/pip install -r requirements.txt
.venv/bin/zensical serve
```

## Docs

Reading order matches `zensical.toml` nav. Use `<div class="run" markdown>` for
command + result pairs (see EventBridge / patina `.run` CSS). No “Looks like”
labels.

## Non-negotiables

1. Stay generic — no Greengrass Thing names, no AWS account IDs, no lab-only
   hostnames as the only path.
2. Dictate **Ubuntu Server** (not Desktop) and OpenSSH on by default.
3. Warn clearly that **Use an entire disk** erases the target drive.
4. Do not commit ISOs or private keys.
