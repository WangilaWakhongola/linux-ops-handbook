# 2. Files & Directories

## Creating and removing

```bash
touch file.txt           # create empty file / update timestamp
mkdir newdir               # create directory
mkdir -p a/b/c               # create nested directories
rm file.txt                   # remove a file
rm -r dir/                     # remove directory recursively
rm -rf dir/                     # force remove, no prompts (use carefully)
rmdir emptydir                   # remove only if empty
```

## Copying and moving

```bash
cp file.txt copy.txt          # copy a file
cp -r dir/ backup/              # copy a directory recursively
mv file.txt newname.txt          # rename / move a file
mv file.txt /tmp/                  # move into another directory
```

## Viewing file contents

```bash
cat file.txt              # print whole file
less file.txt               # paginated view (q to quit)
head -n 20 file.txt           # first 20 lines
tail -n 20 file.txt             # last 20 lines
tail -f /var/log/syslog           # live-follow a growing file (logs!)
```

## Searching

```bash
find / -name "*.conf"           # find files by name
find . -type f -mtime -1          # files modified in the last day
grep "error" file.txt               # search text in a file
grep -r "error" /var/log/             # recursive search in a directory
grep -i "error" file.txt                # case-insensitive
locate nginx.conf                         # fast search using a prebuilt index
```

## File info

```bash
file myfile                  # detect file type
stat file.txt                  # detailed metadata (size, timestamps, inode)
du -sh dir/                      # size of a directory
df -h                               # disk space usage per filesystem
wc -l file.txt                        # count lines in a file
```

## Archiving & compression

```bash
tar -cvf archive.tar dir/         # create a tar archive
tar -xvf archive.tar                # extract a tar archive
tar -czvf archive.tar.gz dir/         # create + gzip compress
tar -xzvf archive.tar.gz                # extract a .tar.gz
zip -r archive.zip dir/                   # zip a directory
unzip archive.zip                           # unzip
```

## Common gotchas

- `rm -rf` has no undo — always confirm the path before running, especially as root.
- `find` and `locate` differ: `locate` is fast but relies on an index (`updatedb`) that may be stale.
- Wildcards (`*`) are expanded by the shell before the command runs — `rm *` in the wrong directory is a classic disaster.
