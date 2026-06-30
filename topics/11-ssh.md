# 11. SSH & Remote Access

## Basic connection

```bash
ssh user@host                  # connect to a remote host
ssh user@host -p 2222             # connect on a non-default port
ssh -v user@host                     # verbose, useful for debugging connection issues
```

## SSH keys (preferred over passwords)

```bash
ssh-keygen -t ed25519 -C "you@example.com"    # generate a new keypair
ssh-copy-id user@host                            # copy your public key to a remote host
cat ~/.ssh/id_ed25519.pub                           # view your public key
```

## SSH config file (saves typing)

```bash
# ~/.ssh/config
Host myserver
    HostName 192.168.1.50
    User emmanuel
    Port 22
    IdentityFile ~/.ssh/id_ed25519
```

```bash
ssh myserver        # now this just works
```

## Copying files

```bash
scp file.txt user@host:/path/         # copy a local file to remote
scp -r dir/ user@host:/path/             # copy a directory recursively
rsync -avz dir/ user@host:/path/            # better for large/repeated transfers
```

## Running remote commands

```bash
ssh user@host "df -h"           # run a single command remotely
ssh user@host "sudo systemctl restart nginx"  # remote service restart
```

## Tunnels & port forwarding

```bash
ssh -L 8080:localhost:80 user@host    # local port forward (access remote service locally)
ssh -R 9000:localhost:3000 user@host     # remote port forward
ssh -D 1080 user@host                       # dynamic SOCKS proxy
```

## Common gotchas

- `Permission denied (publickey)` usually means either the wrong key is being offered or the public key isn't in the server's `~/.ssh/authorized_keys`.
- File permissions on `~/.ssh` matter: the directory should be `700` and `authorized_keys`/private keys `600`, or SSH may silently refuse to use them.
- Disabling password auth (`PasswordAuthentication no` in `/etc/ssh/sshd_config`) is good practice, but confirm key-based login works first or you can lock yourself out.
