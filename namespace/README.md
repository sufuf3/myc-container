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
