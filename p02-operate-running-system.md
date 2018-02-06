### Boot, Reboot and Shutdown a System
* `init` deprecated
* Shutdown
```
# init 0
# shutdown -p ## power off 
# 
```

* Reboot
```
# init 6
# reboot
# systemctl reboot
# shutdown -r +5 System going down for a reboot ## wait 5 min & notification
# shutodnw -r 00:00 ## 00:00 at night
# shutdown -r now
# shutdown -c ## cancel the scheduled task
```

* Halt
```
# halt
# systemctl halt
# shutodwn -h +5
# shutodwn -h now
```

* Poweroff
```
# shutdown -P
```

* Target (Location of unit configuration files )
  - `/usr/lib/systemd/system`

### Boot Systems into Different Targets Manually
**keywords**: boot, target, interrupt, isolate, multi-user.target, graphical.target

#### Targets
* A dependency grouping / Unit configuration file

* Show the target available on system
```
# systemctl list-units --type=target
```
* List all types
```
# systemctl -t help
```
* Location of unit configuration files
  - unit-config-file for **SYSTEM** service `/usr/lib/systemd/system`
  - unit-config-file for **CUSTOMIZE** service `/etc/systemd/system`. **Loads before & Overrides (service with same name) the services in** `/usr/lib/systemd/system`.
* Look inside a unit-config-file
```
### Ex. /usr/lib/systemd/system/sshd.service
[Unit]
Description=OpenSSH server daemon
Documentation=man:sshd(8) man:sshd_config(5)
After=network.target sshd-keygen.service      ## start running only after these services on
Wants=sshd-keygen.service

[Service]
Type=notify
EnvironmentFile=/etc/sysconfig/sshd
ExecStart=/usr/sbin/sshd -D $OPTIONS  ### upon start exec this command
ExecReload=/bin/kill -HUP $MAINPID  ### when ssh reload issue this command
KillMode=process
Restart=on-failure
RestartSec=42s

[Install]
WantedBy=multi-user.target  ### as a dependency of this target
```

* Show dependencies
```
# systemctl list-dependencies multi-user.target
# systemctl list-dependencies graphical.target
# systemctl list-dependencies emergency.target
# systemctl list-dependencies rescue.target
```

* Show current target
```
# systemctl get-default
### get: graphical.target
### or: multi-user.target
```

* `emergency.target`
  - has to login as root
  - mounts the FS as read-only FS
  - single-user environment (with minimum requirements loaded)

* `isolate` command
```
AllowIsolate: Takes a boolean argument. If true, this unit may be used with the systemctl isolate command. 
```
switch between targets
```
# systemctl get-default
# systemctl isolate multi-user.target
# systemctl isolate graphical.target
# systemctl set-default multi-user.target ### changes the symlink of `default.target` file
```

#### Interrupt Boot process
From `graphical.target` to `multi-user.target`/`emergency.target`/`rescue.target`
1. In boot menu select boot item, press `e` (edit)
2. In `grub` configuration, find linux kernel setup section
3. Append `systemd.unit=rescue.target` at the end of kernel setup section
4. `CTRL-X` to continue the boot process

### Interrupt the Boot Process to Gain Access to a System
`grub` allows us to load a local kernel inside of the memory. That kernel have `systemd` and `root` dir.

1. Go into `initramfs`:
  - In `grub` configuration, find linux kernel setup section
  - Append `rd.break` at the end of kernel setup section, `CTRL-X` to continue the boot process
2. Remount `/sysroot` as read&write [command explain](https://askubuntu.com/questions/448231/usage-of-mount-o-remount-rw)
  - `# mount -o remount,rw /sysroot`
3. Change root
  - `# chroot /sysroot`
4. Change password `passwd`
5. Let SELinux relabel everything
  - `# touch /.autorelabel`
6. Finish `# eixt`

### Identify CPU/Memory Intensive Processes, Adjust Process Priority and Kill Processes
**keywords**: cpu, memory, process, kill, priority, nice, renice, `ps`

#### Processes
* `ps`
  - `# ps aux | grep httpd` is equivalent to `# pgrep httpd`
* `pgrep`
  - `# pgrep httpd -l`, `-l` add the processes' names
  - `# pgrep -u chuan -l`, show the processes owned by that user
  - `# pgrep -u chuan -l httpd`
  - `# pgrep -v -u chuan -l`, `-v` = inverse. processes not owned by user
* `pkill`
  - `# pkill httpd`, grep all processes named httpd and kill them
  - `-15`/`-SIGTERM` as default
  - `# pkill -t pts/1` kill by terminal
  - `# pkill -u chuan sshd` kill by user name
* `kill`
  - `# kill -l`, show all the available signals
* **Signals** [link](https://www.gnu.org/software/libc/manual/html_node/Termination-Signals.html)
  - `-15`/`-SIGTERM` nice kill, terminate cleanly, best way (common way)
  - `-9`/`-SIGKILL` kill immediately, kill **malicious processes**, do not allow anything to block the process/cleanup.
  - `-1`/`-SIGHUP` report a controlling process in terminal
  - `-2`/`-SIGINT` keyboard interrupt
  - `-3`/`-SIGQUIT`
  - `-18`/`-SIGCONT`
  - `-19`/`-SIGSTOP` cannot be ignored
  - `-20`/`-SIGTSTP` can be ignored
* Create a dumy process
```
# (while true; do echo -n "my program" >> ~/output.file; sleep 1; done) &  ### jobs running in background
# jobs ### show running jobs
# kill -SIGSTOP %1 ### %1 is the job number
# kill -SIGCONT %1
# kill %1 
```

//++

### Start, Stop and Check the Status of Network Services
```
# systemctl start network.service
# systemctl stop network
# systemctl status network
# systemctl is-enabled network
# systemctl is-enabled network
# systemctl enable network
# systemctl disable network
```

### Securely Transfer Files Between Systems
* `scp` or `sftp` using port 22
```
# scp mydir/myfile username@destination.address:/home/username/
#
# sftp username@destination.address
sftp> ?
sftp> ls
sftp> cd
sftp> get Somefile
sftp> put mydir/myfile
sftp> quit
```