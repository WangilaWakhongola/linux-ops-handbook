# 4. Users & Groups

## Viewing users

```bash
whoami                # current user
id                      # current user's uid/gid/groups
cat /etc/passwd          # list all users on the system
who                         # who is logged in
w                              # logged-in users + what they're doing
last                             # login history
```

## Managing users (requires root/sudo)

```bash
sudo useradd -m emmanuel          # create user with home directory
sudo passwd emmanuel                # set/change password
sudo usermod -aG sudo emmanuel        # add user to sudo group
sudo usermod -L emmanuel                # lock an account
sudo userdel -r emmanuel                  # delete user + home directory
```

## Managing groups

```bash
sudo groupadd devs           # create a group
sudo gpasswd -a emmanuel devs  # add user to a group
sudo gpasswd -d emmanuel devs    # remove user from a group
cat /etc/group                     # list all groups
```

## Switching users / privilege escalation

```bash
sudo command               # run a single command as root
sudo -i                       # interactive root shell
su - emmanuel                   # switch to another user (needs their password)
sudo -u emmanuel command           # run a command as another user
```

## Password & account policy

```bash
chage -l emmanuel           # view password expiry info
sudo chage -M 90 emmanuel     # set password to expire in 90 days
```

## Common gotchas

- `useradd` without `-m` won't create a home directory — `adduser` (Debian/Ubuntu) is more user-friendly and does this automatically.
- Being in the `sudo` group ≠ being root all the time — each `sudo` command still needs your password (cached briefly after first use).
- `/etc/passwd` no longer stores actual password hashes — those live in `/etc/shadow`, readable only by root.
