# 1. Shell & Navigation Basics

The shell is your interface to Linux. Most of IT Ops work happens here, not in a GUI.

## Where am I / what's around me

```bash
pwd                 # print working directory
ls                   # list files
ls -la               # list all (incl. hidden), long format
ls -lh               # human-readable file sizes
```

## Moving around

```bash
cd /var/log          # absolute path
cd ..                 # up one directory
cd ~                  # home directory
cd -                  # previous directory
```

## Getting help

```bash
man ls                # manual page for a command
ls --help             # quick usage help
whatis ls              # one-line description
type ls                # is it a binary, alias, or builtin?
```

## Command history & shortcuts

```bash
history               # list recent commands
!123                   # rerun history item 123
ctrl + r               # reverse search history
ctrl + c               # kill current command
ctrl + d                # exit shell / EOF
tab                     # autocomplete
```

## Redirection & pipes

```bash
command > file.txt        # overwrite file with output
command >> file.txt        # append output to file
command < file.txt          # use file as input
cmd1 | cmd2                  # pipe output of cmd1 into cmd2
command 2> errors.log        # redirect stderr only
command > out.log 2>&1        # redirect both stdout and stderr
```

## Environment variables

```bash
echo $PATH             # show PATH
export VAR=value        # set a variable for this session
env                       # list all environment variables
which python3              # show full path of a command
```

## Common gotchas

- `cd` with no arguments takes you home — useful, but easy to lose your place by accident.
- Tab-completion fails silently if there are multiple matches; press tab twice to list them.
- `>` truncates files instantly with no warning — always double-check before overwriting logs or configs.
