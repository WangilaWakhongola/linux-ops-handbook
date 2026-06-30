# 3. Permissions & Ownership

Linux permissions are central to sysadmin work — most "permission denied" tickets come down to this.

## Reading permission strings

```
-rwxr-xr--  1 emmanuel devs  4096 Jun 30 10:00 deploy.sh
```

- First character: file type (`-` file, `d` directory, `l` symlink)
- Next 3: owner permissions (rwx)
- Next 3: group permissions (r-x)
- Last 3: others permissions (r--)

`r` = read, `w` = write, `x` = execute.

## Changing permissions

```bash
chmod 755 script.sh          # rwxr-xr-x (owner all, group/others read+exec)
chmod 644 file.txt             # rw-r--r-- (owner read/write, others read only)
chmod +x script.sh               # add execute permission
chmod -w file.txt                  # remove write permission
chmod u+x,g-w file.txt               # symbolic: owner +exec, group -write
```

### Numeric (octal) cheatsheet

| Number | Permission |
|--------|-----------|
| 7 | rwx |
| 6 | rw- |
| 5 | r-x |
| 4 | r-- |
| 0 | --- |

Order is always: owner, group, others (e.g. `750` = owner rwx, group r-x, others none).

## Changing ownership

```bash
chown emmanuel file.txt              # change owner
chown emmanuel:devs file.txt           # change owner and group
chown -R emmanuel:devs dir/              # recursive, for whole directories
chgrp devs file.txt                        # change group only
```

## Special permissions

```bash
chmod u+s /usr/bin/somebinary     # setuid - run as file owner
chmod g+s /shared/dir                # setgid - new files inherit group
chmod +t /tmp                          # sticky bit - only owner can delete their files
```

## Checking access

```bash
ls -l file.txt          # see permissions
id                        # show your uid, gid, groups
groups emmanuel             # show groups a user belongs to
sudo -l                       # see what a user can run with sudo
```

## Common gotchas

- `chmod 777` is rarely the right fix — it's a security hole, not a debugging step.
- Directories need execute (`x`) permission to be entered (`cd`), not just read.
- A file can be world-writable but the containing directory's permissions still control who can delete/rename it.
