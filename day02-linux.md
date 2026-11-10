# Day 2: Processes, systemd, and Package Management

## Processes
- A process is a running instance of a program. Each has a unique PID and a parent (PPID).
- Every process descends from PID 1 (systemd on Ubuntu).
- Zombie = finished child whose parent hasn't collected its exit status. Orphan = child whose parent died first (adopted by PID 1).
- Foreground owns the terminal. Background (`&`) returns the prompt.
- Process states in `ps` STAT: R running, S sleeping, D uninterruptible wait, T stopped, Z zombie.

### Commands
    echo $$                         # my shell's PID
    ps aux                          # all processes
    ps -ef                          # all processes with PPID
    pstree -p | head                # process tree
    pgrep -a sleep                  # find PIDs by name
    sleep 300 &                     # start in background
    jobs                            # list jobs
    # Ctrl+Z pauses, bg resumes in background, fg brings to foreground
    nohup sleep 300 &               # survives terminal close
    top                             # live view (q to quit)
    htop                            # friendlier top
    ls /proc/<PID>/                 # status, cmdline, fd/

## Signals
- A signal is a small message sent to a process. It can be caught, ignored, or trigger the default action. SIGKILL and SIGSTOP can't be caught.
- SIGTERM (15): polite "please exit", default for `kill`. Lets the program clean up.
- SIGKILL (9): forced, no cleanup. Can leave lock files and corrupted data.
- SIGINT (2): Ctrl+C. SIGTSTP (20): Ctrl+Z. SIGHUP (1): terminal closed or reload config. SIGCONT (18): resume.
- Signals come from the keyboard, commands (`kill`, `pkill`, `systemctl stop`, `docker stop`), the kernel (SIGSEGV, OOM killer), and other programs.
- Exit code of a signalled process = 128 + signal number (137 = SIGKILLed).

### Commands
    kill <PID>              # SIGTERM
    kill -9 <PID>           # SIGKILL
    kill -STOP <PID>        # pause
    kill -CONT <PID>        # resume
    kill -0 <PID>           # check the process exists
    pkill sleep             # by name
    kill -l                 # list signals

## systemd
- systemd is PID 1: starts services at boot, restarts them if they crash, handles dependencies, collects logs.
- Everything is a unit: .service, .timer, .socket, .mount, .target.
- Unit files: `/etc/systemd/system/` (mine), `/usr/lib/systemd/system/` (from packages).
- `start`/`stop` change the state now. `enable`/`disable` change whether it starts at boot. They are independent.
- After creating or editing a unit file, run `daemon-reload`.

### Commands
    systemctl status nginx
    sudo systemctl start|stop|restart|reload nginx
    sudo systemctl enable|disable nginx
    sudo systemctl enable --now nginx
    systemctl is-active nginx
    systemctl is-enabled nginx
    systemctl --failed
    systemctl cat nginx
    sudo systemctl daemon-reload

## Logs (journalctl)
    journalctl -u nginx             # logs for a service
    journalctl -u nginx -n 50       # last 50 lines
    journalctl -u nginx -f          # follow live
    journalctl -u nginx --since "10 min ago"
    journalctl -p err -b            # errors this boot
    journalctl -xe                  # recent logs with explanations
- Some services also write files in `/var/log/<service>/`.

## apt (package management)
    sudo apt update                 # refresh package lists
    sudo apt install nginx -y
    sudo apt remove nginx
    apt search <name>
    dpkg -l | grep nginx            # installed packages

## Lab: nginx
    sudo apt install nginx -y
    systemctl status nginx
    curl localhost
    sudo systemctl stop nginx
    sudo systemctl disable nginx
    sudo systemctl enable --now nginx
    journalctl -u nginx -n 20

What I observed: (fill in)

## Lab: my own service (hello.service)
Script `~/lab/app/hello.sh`: (paste your script)

Unit file `/etc/systemd/system/hello.service`: (paste your unit file)

    sudo systemctl daemon-reload
    sudo systemctl enable --now hello
    journalctl -u hello -f

## Break/fix: broken.service
Debug loop: status, then logs, then change one thing, then daemon-reload, then restart.

### Bug 1
- Symptom: (fill in)
- Evidence: (fill in)
- Cause: (fill in)
- Fix: (fill in)

### Bug 2
- Symptom: (fill in)
- Evidence: (fill in)
- Cause: (fill in)
- Fix: (fill in)

### Bug 3
- Symptom: (fill in)
- Evidence: (fill in)
- Cause: (fill in)
- Fix: (fill in)

Lesson: (fill in)

## Self-check answers
1. kill vs kill -9: (fill in)
2. systemctl start vs enable: (fill in)
3. Where to find service logs: (fill in)

## Problems I hit
- (fill in)
EOF
