# 5. Processes & Jobs

## Viewing running processes

```bash
ps aux                  # all processes, BSD-style listing
ps -ef                    # all processes, full format
top                          # live process viewer
htop                           # nicer live viewer (may need install)
pgrep nginx                      # find PID(s) by process name
pidof nginx                        # same idea, just PIDs
```

## Killing processes

```bash
kill 1234              # send SIGTERM (graceful stop) to PID 1234
kill -9 1234              # send SIGKILL (force kill, no cleanup)
killall nginx                # kill all processes by name
pkill -f "python app.py"       # kill by matching command line
```

## Foreground vs background jobs

```bash
long_command &           # run in background
jobs                        # list background jobs in this shell
fg %1                         # bring job 1 to foreground
bg %1                           # resume job 1 in background
ctrl + z                          # suspend current foreground job
nohup long_command &                # keep running after terminal closes
disown                                # detach job from shell
```

## Process priority

```bash
nice -n 10 command           # start a process with lower priority
renice -n 5 -p 1234              # change priority of a running process
```

## Resource usage

```bash
top                    # CPU/memory live view
free -h                   # memory usage, human-readable
uptime                       # load average + how long system has been up
vmstat 1                       # system performance snapshot, every 1s
```

## Common gotchas

- `kill -9` skips cleanup (no closing files, no graceful shutdown) — try plain `kill` first.
- A process surviving terminal close needs either `nohup`, `disown`, or to run under a proper service manager (see systemd guide).
- High load average isn't always CPU-bound — check `vmstat`/`iostat` too, since I/O wait counts toward load.
