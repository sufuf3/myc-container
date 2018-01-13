# UTS Namespace

## Execute:
```sh
$ cd namespace/
$ gcc uts.c -o urs.o
$ sudo ./urs.o
```
## play with UTS Namespace
- Get the pid from program
```
# echo $$
11155
```

- Get the ps from host(my computer)
```sh
$ pstree -pl
bash(6722)───sudo(11146)───uts.o(11149)───bash(11155)
```

- Check the namespace type and inode number in program 
(ref: http://man7.org/linux/man-pages/man7/namespaces.7.html)
```sh
# readlink /proc/$$/ns/uts
uts:[4026532211]
# readlink /proc/9121/ns/uts
uts:[4026532211]
# readlink /proc/11149/ns/uts
uts:[4026531838]
```
Memo: `# readlink /proc/11149/ns/uts` is chaeck the child process is different from program process.

- Play the hostname with child process `#` & host `$`
```sh
# hostname -b mylittlecontainer
# hostname
mylittlecontainer
```
```sh
$ hostname
build-container
```

# IPC Namespace
- Run program
```sh
build-container ~/myc-container/namespace # readlink /proc/$$/ns/uts /proc/$$/ns/ipc
uts:[4026532211]
ipc:[4026532212]
build-container ~/myc-container/namespace # ipcs -q

------ Message Queues --------
key        msqid      owner      perms      used-bytes   messages    

build-container ~/myc-container/namespace # ipcmk -Q
Message queue id: 0
build-container ~/myc-container/namespace # ipcs -q

------ Message Queues --------
key        msqid      owner      perms      used-bytes   messages    
0xbee653a6 0          root       644        0            0           

build-container ~/myc-container/namespace # ./ipc.o 
build-container ~/myc-container/namespace # ipcs -q

------ Message Queues --------
key        msqid      owner      perms      used-bytes   messages
```
# PID Namespace

`bash(6722)───sudo(19328)───pid.o(19329)───bash(19330)`
```sh
build-container ~/myc-container/namespace # echo $$
1
```

# Mount Namespace
```sh
build-container ~/myc-container/namespace # ls /proc
ls: cannot read symbolic link '/proc/self': No such file or directory
ls: cannot read symbolic link '/proc/thread-self': No such file or directory
acpi/      cpuinfo    execdomains  ioports    kmsg         mdstat   net@          self@     sysrq-trigger  version
buddyinfo  crypto     fb           irq/       kpagecgroup  meminfo  pagetypeinfo  slabinfo  sysvipc/       version_signature
bus/       devices    filesystems  kallsyms   kpagecount   misc     partitions    softirqs  thread-self@   vmallocinfo
cgroups    diskstats  fs/          kcore      kpageflags   modules  sched_debug   stat      timer_list     vmstat
cmdline    dma        interrupts   keys       loadavg      mounts@  schedstat     swaps     tty/           zoneinfo
consoles   driver/    iomem        key-users  locks        mtrr     scsi/         sys/      uptime
build-container ~/myc-container/namespace # [01/13@10:39:38 1]ps -ef
Error, do this: mount -t proc proc /proc
build-container ~/myc-container/namespace # [01/13@10:39:56 47]mount -t proc proc /proc
build-container ~/myc-container/namespace # ls /proc
1/         consoles   execdomains  irq/         kpagecount  modules       schedstat  sys/           version
67/        cpuinfo    fb           kallsyms     kpageflags  mounts@       scsi/      sysrq-trigger  version_signature
acpi/      crypto     filesystems  kcore        loadavg     mtrr          self@      sysvipc/       vmallocinfo
buddyinfo  devices    fs/          keys         locks       net@          slabinfo   thread-self@   vmstat
bus/       diskstats  interrupts   key-users    mdstat      pagetypeinfo  softirqs   timer_list     zoneinfo
cgroups    dma        iomem        kmsg         meminfo     partitions    stat       tty/
cmdline    driver/    ioports      kpagecgroup  misc        sched_debug   swaps      uptime
build-container ~/myc-container/namespace # ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 10:39 pts/3    00:00:00 /bin/bash
root        69     1  0 10:40 pts/3    00:00:00 ps -ef
```
- On host
```sh
ufuf3@build-container ~ $ ls /proc
ls: cannot read symbolic link '/proc/self': No such file or directory
ls: cannot read symbolic link '/proc/thread-self': No such file or directory
acpi/      cpuinfo    execdomains  ioports    kmsg         mdstat   net@          self@     sysrq-trigger  version
buddyinfo  crypto     fb           irq/       kpagecgroup  meminfo  pagetypeinfo  slabinfo  sysvipc/       version_signature
bus/       devices    filesystems  kallsyms   kpagecount   misc     partitions    softirqs  thread-self@   vmallocinfo
cgroups    diskstats  fs/          kcore      kpageflags   modules  sched_debug   stat      timer_list     vmstat
cmdline    dma        interrupts   keys       loadavg      mounts@  schedstat     swaps     tty/           zoneinfo
consoles   driver/    iomem        key-users  locks        mtrr     scsi/         sys/      uptime
sufuf3@build-container ~ $ [01/13@10:39:46 1]ls /proc
ls: cannot read symbolic link '/proc/self': No such file or directory
ls: cannot read symbolic link '/proc/thread-self': No such file or directory
1/         consoles   driver/      iomem     key-users    locks    mtrr          scsi/     sys/           uptime
acpi/      cpuinfo    execdomains  ioports   kmsg         mdstat   net@          self@     sysrq-trigger  version
buddyinfo  crypto     fb           irq/      kpagecgroup  meminfo  pagetypeinfo  slabinfo  sysvipc/       version_signature
bus/       devices    filesystems  kallsyms  kpagecount   misc     partitions    softirqs  thread-self@   vmallocinfo
cgroups    diskstats  fs/          kcore     kpageflags   modules  sched_debug   stat      timer_list     vmstat
cmdline    dma        interrupts   keys      loadavg      mounts@  schedstat     swaps     tty/           zoneinfo
```

# User Namespace
```sh
$ sudo id
uid=0(root) gid=0(root) groups=0(root)
$ sudo ./user.o 
nobody@build-container ~/myc-container/namespace $ id
uid=65534(nobody) gid=65534(nogroup) groups=65534(nogroup)
nobody@build-container ~/myc-container/namespace $ exit
```

# Coding Style
- K & R
- tool: astyle
- cmd: `astyle --style=kr <filename>`
