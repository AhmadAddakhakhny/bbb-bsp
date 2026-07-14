# System-D

##### What does init manager do - systemd?
> system boot initialization
> service management
> dependancy resolution
> process monitoring, restart, shutdown and reboot.

##### What does init process mean?
> normally, after the kernel finishing its initialization, it gives the control to the init process (user-space)

##### init process criteria?
> where the init-binary resides
> what does it do, right after boot-up

##### What is system-d?
> it's a service management tool (suit of tools) to be able to control all of my services.
> systemd shall be considered as super kernel, as it provides things for user that should only be provided by the kernel side.

##### Explain system-d architecture?
```
1. Commands:
    systemctl   -   journalctl     -    notify  -   analyze     
ps. commands shall be handled by the below daemons.

2. Daemons                             ||       2`. targets ==> states = run level
    systemd -   journald    - networkd ||       basic   -   multi-user  -   reboot  -   graphical

3. core
    * manager                   * units: these are special files
    * namespaces                        services    path
    * systemd                           timers      sockets
    * cgroups                           target

4. libraries:
    libnotify       dbus-1      libpam

5. Kernel:
    cgroups     autofs      kdbus

Note: as a developer mostly your interaction would be with the commands and units.
```

##### How to find the location of the binary of systemd?|
> run $ sudo realpath /proc/1/exe

##### Where are the configuration files of systemd resides?
> under /etc/systemd
ps: the file system.conf contains keys that can be set to configure the launch of systemd, and these keys shall be overwritten via unit files.


##### what can systemd launch?
> it can load kernel modules
> it can start services and check their dependanies, and also run in parallism in case other services aren't dependant.

----
### Units
> there are config files for services and descripes their behaviours.
> note: their path under /lib/systemd/system

> 1. Service unit
> 2. Socket Unit
> 3. Path Unit
> 4. Target Unit

###### Service Unit
> * Definition: Normally, there are used to launch user-space services based upon the info inside unit.service file, withere these services are default ones shiped with the distro, or a user wrote it him self.  
> * Dependancy: Explain the dependancies of the service.  
> * Path: Show the path of the executable of the service.  
> * Kill: how this service shall be killed. (kill it only, or kill it with the spinned off childs) horror!  
> * Restart: definining how shall restart take place for this service (service was killed by watchdog, shall we restart it or not ... etc) look at the docs for more info.
> * privelge: define service privelages if any.  
> * target: in which target this service is intalled.  
> P.S. to understand any key-value pair and the options that shall be assigned to these keys, please look at the systemd documentations on google. example: systemd.service, systemd.exec, systemd.kill.  
> tip: you can source some envirnment variables from the xyz.service unit.

```shell
# a snap shot of a unit.service script structure
[Unit]
Description=xyz

[Service]
ExecStartPre=/usr/sbin/xyz

[Install]
wantedBy=multi-user.target
```

> More in Dependancies:
```shell
[Unit]
# Prepherablly run these service/s if possible [weak]
Wants=app1.service app2.service

# It's a must to operate these service/s [strong]
Requires=network.service

# Run the current service after running the below service/s with the specified order (order matters)
After=can.service

# Run the current service before running the below service/s
Before=connman.service
```
> Note: **Install** section sometimes holds a key-value like, **WantedBy=multi-user.target**, simply it means please install the current unit to be part of **multi-user.target**. A target is composed out of multible units, and there is a default target is assigned to system-d and at startup it launches all units inside the default target.
###### How to create a new service?
> $ sudo systemctl edit --force --full <service_name>.service  

----

### Socket unit
> Definition: it is used to create a network socket, controls service that needs a socket.  
> Example, I can reate a socket and assign it to a service. the default state of the service is down, and whenever there is activity on the socket, the systemd would run the assign service.  
> Note: sockets interact only with tcp/udp layer (logical speaking)   
> P.S.: both the socket unit and service unit must have the same prefix!  
```shell
# a snap shot of a unit.socket script structure
[Unit]
Description=xyz

[Socket]
[Socket]
ListenStream=22

[Install]
wantedBy=sockets.target
```

----

### Path unit
> Definition: it's a string represents location on root-filesystem.
> systemd will keep monitoring this file/directory location and if noticed any activity overther, it would run a specific service.  
> P.S.: both the socket unit and service unit must have the same prefix!  

```shell
# a snap shot of a unit.path script structure
[Unit]
Description=xyz

[Path]
DirectoryNotEmpty=/etc/xyz/xxx

[Install]
wantedBy=paths.target
```
----

### Target unit
> Target = State = it's a group of units
> the systemd should at least run a target or be in a specific state.
> $ sudo systemctl get-default: tells you the default target/state of systemd.

----

### Commands
> As a developer what shall I do:
    - verifiy status of service  
    - Starting/Stoping/restarting/Reloding service  
    - Enabling/Disabling/killing service  
    - Masking service  

###### Verify service status
> $ systemctl status ssh 

###### Starting/Stoping/Restarting/Reloding service
> $ systemctl start ssh  # Execstart
> $ systemctl stop ssh   # Execstop or kill signal
> $ systemctl restart ssh # stop= then start
> $ systemctl reload ssh # ExecReload= then Execstart or Execstart

###### Enabling/Disabling/killing service
> if a unit is diabled, it will be execluded from the multi-service.target, which meand at startup systemd won't run this service.    
> $ systemctl enable --now <service unit>  
> $ systemctl disable --now <service unit>  
> $ sudo systemctl kill <service unit>  # the killing will me with the kill mode to avoid having orphan srvices  

##### How to know how a specific service existed?
> $ systemctl status service-name (you would see the exited status code)  
> note: running with sudo give you more info  
----
### Target unit
> the purpose of target units is to customize the user-space, customize the services that should run on the userspace.  
> how to list all targets: $ systemctl list-units --type=target  
> how to check the installed units of a specific target: **$ls /etc/systemd/system/<target-name>.target.wants**
> how to check deafault target: **$systemctl get-default**  
> how to list all units of a target by cmd: **$ systemctl list-dependencies --all graphical.target** - it's better to be used rather than listing the content of **.wants dir.** because the command shows all dependencies for all units inside the target.  
> how to change default targeet: **$ sudo systemctl set-default <unit-name>.target**  
> file structure:
```bash
[Unit]
Description=tbd
```
---
### System-d booting sequence
> $ systemd-analyze                     # shows the boot-up time from power-up until the userspace  
> $ systemd-analyze critical-chain      # show boot-up time for each target  
> $ systemd-analyze blame               # show boot-up time for each service  
---
### Miscl.
> if you want to control a feature of a specific resource, as preventing ssh access for a non-white listed IPs. you should refer to the systemd documentation named "systemd.resource-control.  
> how to log into the journal? echo "Hello, world! | systemd-cat  
---

### Intigrate Systemd with Yocto

###### What are the init managers that Yocto project supports and what is its defualt?
> Yocto project uses initV as a default init manager.  
> it supports busybox, and systemd as well.  

###### How to intigrate systemd with yocto images?
```bash
# Enable systemd in the distro features
DISTRO_FEATURES:append = " systemd"

# Set systemd as the init manager
VIRTUAL-RUNTIME_init_manager = "systemd"

# Prevent sysvinit from being automatically added (backfill)
DISTRO_FEATURES_BACKFILL_CONSIDERED += "sysvinit"

# Replace legacy initscripts with systemd-compatible units
VIRTUAL-RUNTIME_initscripts = "systemd-compat-units"

# Use systemd-journald for logging (no standalone syslog daemon)
VIRTUAL-RUNTIME_syslog = "systemd-journald"

# Disable any fallback base-utils syslog (safe to leave empty)
VIRTUAL-RUNTIME_base-utils-syslog = ""
```