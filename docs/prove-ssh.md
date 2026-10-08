# Prove SSH

From the **workstation** (replace user and IP):

<div class="run" markdown>

```bash
ssh ubuntu@192.168.1.50 'uname -a && . /etc/os-release && echo "$PRETTY_NAME"'
```

```text {.no-copy}
Linux ubuntu-server 6.8.… x86_64 …
Ubuntu 24.04.… LTS
```

</div>

Accept the host key on first connect, then enter the password you set at
install (password auth was enabled in Subiquity). When this works, the host is
ready for whatever lab comes next.

!!! note "Consuming labs"
    Record `HOST_USER` / `HOST_IP` (or equivalent) in that project’s env file.
    Package installs and services stay in that walkthrough.

## Done

You have an SSH-reachable Ubuntu Server host. See
[Troubleshooting](troubleshooting.md) if something failed.
