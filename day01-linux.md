# Day 1: Linux Foundations

## What I learned
- Linux has one directory tree starting at `/`.
  - `/etc` config, `/var` changing data and logs, `/home` user folders,
    `/usr` programs, `/tmp` temporary files, `/proc` live kernel and process info.
- Permissions use r (4), w (2), x (1). `755` = rwxr-xr-x, `644` = rw-r--r--, `600` = rw-------.
- On a directory, `x` means "can enter it", `r` means "can list it", `w` means "can create or delete inside it".

## Commands I ran

### Build the folder tree
    mkdir -p ~/lab/{app,logs,config,backup}
    tree ~/lab

### Private config file
    touch ~/lab/config/app.conf
    chmod 600 ~/lab/config/app.conf
    ls -l ~/lab/config

### Fake log file and live watching
    for i in {1..1000}; do echo "$(date) INFO request $i" >> ~/lab/logs/app.log; done
    wc -l ~/lab/logs/app.log
    tail -f ~/lab/logs/app.log

### Find files
    find ~/lab -type f -size +1k -mmin -60

### Second user with access to app/ only
    sudo useradd -m -s /bin/bash labuser
    chmod o+x ~
    chmod 755 ~/lab ~/lab/app
    chmod 700 ~/lab/config
    sudo -u labuser cat ~/lab/app/hello.txt      # works
    sudo -u labuser cat ~/lab/config/app.conf    # Permission denied

## Problems I hit and how I fixed them
- `Permission denied` on `/etc/shadow`: it is root-only, so use `sudo`.
- `userdel: mail spool not found`: harmless warning, the user was still removed.
- Git commit warning about identity: fixed with `git config --global user.name` and `user.email`.
- `git push` showed nothing new: there were no file changes to commit.

## Break/fix
- Broke permissions with `chmod -R 000 ~/lab`.
- Recovered with: (fill in what you did)

## Self-check answers
1. 644 vs 755: (fill in)
2. Why logs live in /var/log: (fill in)
3. What `find / -name "*.conf" -size +1k 2>/dev/null` does: (fill in)
