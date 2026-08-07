# Linux Interview Questions and Answers

## 1. What is Linux?

Linux is an open-source, Unix-like operating-system kernel. Linux distributions combine the kernel with system utilities, package managers, libraries, shells, and applications.

## 2. What is the difference between the Linux kernel and a distribution?

The kernel manages hardware, processes, memory, devices, filesystems, and networking. A distribution packages the kernel with user-space tools and a software-management ecosystem.

## 3. Describe the Linux boot process.

The typical sequence is:

1. BIOS or UEFI initializes hardware.
2. The bootloader, commonly GRUB, loads the kernel.
3. The kernel initializes memory, drivers, and devices.
4. The initramfs provides temporary startup files.
5. The real root filesystem is mounted.
6. `systemd` or another init system starts services and targets.

## 4. What is a shell?

A shell is a command interpreter that provides an interface to the operating system. Examples include Bash, Zsh, Ksh, and Fish.

## 5. What is the difference between a shell and a terminal?

A shell interprets and executes commands. A terminal is the interface or program through which a user interacts with the shell.

## 6. What is a process?

A process is a running instance of a program. It has a process ID, address space, open file descriptors, environment, security context, and scheduling state.

## 7. What is the difference between a process and a thread?

Processes normally have separate address spaces. Threads within a process share memory and resources but have their own execution stacks and scheduling state.

## 8. What are parent and child processes?

A process that creates another process is the parent. The created process is the child. The child receives a unique PID and normally inherits selected attributes from its parent.

## 9. What are zombie and orphan processes?

A zombie has exited, but its parent has not yet collected its exit status. An orphan is a running process whose parent exited; it is adopted and eventually managed by an appropriate system process.

## 10. How do you list running processes?

Common commands include:

```bash
ps aux
ps -ef
top
htop
ps provides a snapshot, while top and htop provide interactive views.
11. How do you identify a process consuming excessive CPU?
Use top, htop, ps, or pidstat to identify the PID and CPU usage. Then inspect its threads, logs, system calls, workload, configuration, and recent changes.
12. How do you investigate high memory usage?
Check free, vmstat, top, ps, /proc, and application metrics. Determine whether memory is used by processes, page cache, kernel structures, shared memory, or swapping.
13. What is load average?
Load average represents the average number of tasks running or waiting for CPU or uninterruptible resources over approximately 1, 5, and 15 minutes.
14. How should load average be interpreted?
Compare it with the number of available CPU cores and examine CPU, I/O, and task states. A high load average does not always mean CPU saturation because tasks waiting on I/O can contribute.
15. What is virtual memory?
Virtual memory gives processes isolated logical address spaces. The kernel maps virtual addresses to physical memory or other backing storage and enforces memory protection.
16. What is swap?
Swap is disk-backed space used for memory pages that are not currently held in RAM. It can help handle memory pressure, but heavy swapping usually causes major performance degradation.
17. What is the Linux OOM killer?
When the system cannot satisfy memory allocations, the kernel may terminate one or more processes to recover memory. The selected victim is influenced by memory usage and OOM scoring.
18. How do you check memory and swap usage?
Common commands include:
free -h
vmstat 1
swapon --show
cat /proc/meminfo
19. What is a filesystem?
A filesystem defines how files and directories are stored, named, organized, protected, and retrieved on a storage device or logical volume.
20. What is an inode?
An inode stores file metadata such as type, permissions, ownership, timestamps, link count, size, and pointers to data blocks. The filename is stored in a directory entry.
21. Can a filesystem be full when df shows free space?
Yes. It may have exhausted its available inodes. Check block usage with df -h and inode usage with df -i.
22. How do you investigate a full filesystem?
Use df to identify the filesystem and du to locate large directories. Also check deleted-but-open files, logs, package caches, temporary files, container data, snapshots, and inode exhaustion.
23. Why can disk space remain occupied after deleting a file?
A running process may still have the deleted file open. The storage is released only after the final file descriptor is closed or the process exits.
A common diagnostic command is:
lsof +L1
24. What is the difference between a hard link and a symbolic link?
A hard link is another directory entry for the same inode. A symbolic link is a separate file containing a path to another file or directory.
25. What are standard Linux file permissions?
Permissions are read, write, and execute for the file owner, group, and others. They can be displayed using ls -l and modified using chmod.
26. What does permission 755 mean?
It means:
Owner: read, write, and execute.
Group: read and execute.
Others: read and execute.
27. What is umask?
umask removes permission bits from the default permissions used when new files and directories are created. It helps define secure defaults.
28. What are SUID, SGID, and the sticky bit?
SUID makes an executable run with the file owner’s effective identity.
SGID makes an executable use the file group’s identity; on directories, new files inherit the directory group.
The sticky bit on a shared directory limits deletion to the file owner, directory owner, or privileged users.
29. How do you find recently modified files?
For example:
find /var/log -type f -mtime -1
This finds files modified during the applicable previous-day interval. Minute-level searches can use -mmin.
30. How do you find large files?
For example:
find / -xdev -type f -size +1G -print
Limit the search to the intended filesystems and handle permission errors appropriately.
31. What is a file descriptor?
A file descriptor is a process-local integer representing an open file, socket, pipe, device, or similar I/O resource.
32. How do you investigate file-descriptor exhaustion?
Check process and system limits using ulimit, /proc, and sysctl. Use lsof or /proc/<pid>/fd to identify open descriptors and determine whether the application is leaking them.
33. What are Linux signals?
Signals are asynchronous notifications sent to processes. Common examples include SIGTERM for graceful termination, SIGKILL for forced termination, SIGHUP for reload behavior, and SIGINT for interactive interruption.
34. What is the difference between SIGTERM and SIGKILL?
SIGTERM can be handled by a process, allowing cleanup. SIGKILL cannot be caught or ignored and immediately terminates the process through the kernel.
35. What is systemd?
Systemd is a system and service manager. It manages services, dependencies, startup targets, timers, sockets, mounts, logging integration, and other operating-system units.
36. How do you troubleshoot a failed systemd service?
Use:
systemctl status service-name
journalctl -u service-name
systemctl cat service-name
Then check configuration, permissions, dependencies, environment, ports, resource limits, and recent changes.
37. What is journalctl?
journalctl queries logs collected by the systemd journal. It can filter by unit, boot, time, priority, process, and other structured fields.
38. What is the difference between cron and systemd timers?
Both schedule work. Systemd timers integrate with service units, dependency management, centralized logs, missed-run handling, and resource controls. Cron is simpler and remains widely used.
39. How do you identify which process is listening on a port?
Use:
ss -lntp
lsof -i :PORT can also provide useful process information when available.
40. How do you test connectivity to a remote service?
Check name resolution and the network path, then test the application port:
dig example.com
ip route
ping host
traceroute host
nc -vz host 443
curl -v https://host
Select commands according to protocol and environment restrictions.
41. How does DNS resolution work on Linux?
Applications use the configured resolver behavior, commonly influenced by /etc/nsswitch.conf, /etc/resolv.conf, local caching services, search domains, and configured DNS servers.
42. What is the difference between 127.0.0.1, 0.0.0.0, and a host IP?
127.0.0.1 is loopback and is reachable only locally. Binding to 0.0.0.0 listens on all available IPv4 interfaces. Binding to a specific host IP listens only on that interface.
43. How do you troubleshoot Linux network connectivity?
Check interface state, IP addresses, routes, DNS, neighbor resolution, listening services, host firewall, remote firewall, security groups, proxies, MTU, packet loss, and packet captures.
44. What do grep, sed, and awk do?
grep searches text for matching patterns.
sed performs stream-oriented text transformations.
awk processes structured text using fields, patterns, and actions.
45. What is a pipe?
A pipe sends the standard output of one command to the standard input of another:
producer | consumer
It enables small commands to be composed into processing pipelines.
46. Explain standard input, output, and error.
The standard file descriptors are:
0: standard input
1: standard output
2: standard error
They can be redirected independently.
47. What is the difference between > and >>?
> creates or truncates a file before writing. >> appends output to the existing file.
48. What is an exit status?
An exit status is the integer returned when a command terminates. By convention, zero indicates success and a nonzero value indicates some form of failure.
The previous status is available in Bash as:
$?
49. How do you inspect system calls made by a process?
Use strace to trace system calls and signals:
strace -p PID
It is useful for investigating file, network, permission, and blocking behavior but adds overhead and may expose sensitive values.
50. A Linux server is slow. How would you investigate it?
Establish when the issue started and what changed. Check CPU, load, memory, swap, disk capacity, disk latency, network behavior, process states, logs, service health, resource limits, and external dependencies before forming a root-cause hypothesis.
    Useful commands include:
    uptime
    top
    free -h
    vmstat 1
    iostat -xz 1
    df -h
    df -i
    ss -s
    journalctl -p warning