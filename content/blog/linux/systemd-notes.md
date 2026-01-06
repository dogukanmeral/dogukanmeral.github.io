+++
author = "Doğukan Meral"
title = "systemd notes"
date = "2026-01-06"
description = "systemd notes and some commands"
tags = [
    "systemd",
    "services",
    "cli",
]
categories = [
    "linux",
]
+++

# systemd

## init

- kernel starts init directly
- init starts everything else (forks)
- has some power over other process, **eg.** takes care of orphan, zombie processes

---

## made of

 lots of smaller programs / services / libraries
 - systemctl
 - journalctl
 - Init
 - Process management
 - Network management: **networkd**
 - Login management: **logind**
 - Logs: **journald**
 - etc....

---

## systemd units

Any entity managed by systemd
- Service
- Socket
- Device
- Mount point or auto-mount point
- Swap file
- Partition
- Startup target
- Watched filesystem path
- Group of externally created processes

---

## Unit file locations

- /lib/systemd/system  -  standart systemd unit files (distro maintainers)
- /usr/lib/systemd/system  -  from locally installed packages
- /run/systemd/system  -  transient unit files
- /etc/systemd/system  -  custom unit files  -  **highest priority**

---

## Example unit file

```
[Unit]
Description=The NGINX HTTP and reverse proxy server
After=network.target remote-fs.target nss-lookup.target
Wants=network-online.target

[Service]
Type=forking
PIDFile=/run/nginx.pid
ExecStartPre=/usr/sbin/nginx -t -q -g 'daemon on; master_process on;'
ExecStart=/usr/sbin/nginx -g 'daemon on; master_process on;'
ExecReload=/usr/sbin/nginx -g 'daemon on; master_process on;' -s reload
ExecStop=/bin/kill -s QUIT $MAINPID
TimeoutStopSec=5
KillMode=mixed

[Install]
WantedBy=multi-user.target
```

`systemctl daemon-reload`: force systemd to re-read files (**does not restart**)

---

## systemctl

`systemctl list-units --type=service`

`systemctl list-unit-files`

`systemctl status`: status of service

`systemctl restart`: stop and start combined (**starts** if it was stopped)

> `type=forking` requires a PID file to main process

### Unit states

- inactive: (deactivating, exited)
- active: (activating, running)
- failed

- static (not started, frozen by systemd)
- bad (broken): usually syntax problems on unit file
- masked: exists on disk but systemd ignores it
- indirect: disabled but another unit file referances it
- linked

---

## Targets

Target units are used to group units and to set synchronization points for ordering dependencies with other unit files.

`systemctl list-units --type=target`

`systemctl get-default`: last target to reach

`systemctl add-wants multi-user.target nginx.service`: multi-user target includes nginx (DO this IN **unit file**, keep things in unit files as much as possible)

---

## Dependencies & ordering

### Wants, wantedby

**Weakest dependency**: Activate together, but no big deal if you don't (**NO guarantee**)

```
/lib/systemd/system/friendly-recovery.target
Wants=friendly-recovery.service
```

```
/lib/systemd/system/motd-news.timer
WantedBy=timers.target
```

### Requires, RequiredBy

**Strong dependency**: If a pre-requisite fails, don't bring up (units must be activated together)

```
/lib/systemd/system/multi-user.target
Requires=basic.target
```

```
/lib/systemd/system/systemd-boot-check-no-failures.service
RequiredBy=boot-complete.target
```

> Still weaker than **explicit ordering**

### Explicit ordering (Execution order)

"In what order are units started?"
```
/lib/systemd/system/console-getty.service
Before=getty.target
```

```
/lib/systemd/system/motd-news.service
After=network-online.target
```

### Others

- Requisite  -  like Requires, but must **already** be active
- BindsTo  -  like Requires but more tightly coupled
- PartOf  -  like Requires, but only affects starting/stopping
- Conflicts  -  exclude units from being active simultaneously

## journalctl

reading logs of systemd

`journalctl`: all the logs until OS installation

`journalctl -r`: reverses the order, latest logs first

`journalctl -f`: live logs

`journalctl -u <service>`: filter logs by service

`journalctl -b`:  current boot logs