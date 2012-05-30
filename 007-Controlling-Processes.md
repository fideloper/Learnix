# Controlling Processes

A process is a running program and allows management of program's use of memory, processsor time, and I/O resources.

The kernel tracks each process, including processes:

* address space map
* status (sleeping, stopped, runnable, etc)
* execution priority [Determine how much CPU time a process receives]
* resources used
* files/ports opened
* signal mask
* owner

Processes can have threads - a fork in the execution of the process. This works best with multi-core CPUs.

### PID: process ID number
Every process has a unique ID which are assigned in the order they are created.

### PPID: parent PID
Existing programs must clone themselves to create new processes. Cloned programs (child programs) are assigned a PPID to signify which program was its parent. This is useful for debugging, as it gives you a clue as to its purposes (For instance, a program run from shell is probably a user-run program).

### UID and EUID: real and effective user ID
UID is the user ID number of the person who created it (A copy of the UID value of the parent process).

The EUID is an extra UID used to determine what resources and files a process has permission to access.

Its useful to maintain a distinction between identity and permissions.

### GID and EGID: real and effective group ID
Group identificatino number, acting the same as UID and EUID. Group permissions are mostly taken into account for creating new files.

## Process Life Cycle
A process copies itself with **fork**. After forking, a process will often use **exec** to reset its state, thus becoming different from the parent.
When a process is ready to die, it calls **_exit**. Before the process disappears, it waits for the parents acknowledgement, via **wait**. The parent receives the childs exit code (usually 0 for successful, other for error). If the parent has died before the child, however, the processes' parent becomes **init** which is alwasy process number 1.

## Signals
Signals are process-level interrupt requests. Think waiting for user-input, or cancelling a processes with ctrl+C.

* Sent among processes to communicate
* Sent in terminal via ctrl+c or ctrl+z
* Sent by admin, via **kill** command for instance
* Sent to kernel for infractions such as DBZ
* Can be send to kernel to notify of events such as death of child process.

When a signal is received, a method in the process may handle it, or the kernel may take action on behalf of the process.

Signals can be ignored, blocked queued or other. 

	kill -l #List of available signales
	nano /usr/include/signal.h #More specific info
	man signal #More specific info

* Kill - unblockable and terminates a process at the kernel level
* INT - sent when using ctrl+C. Request to terminate. If not caught, it kills the process by default.
* TERM - a request to terminate execution. Expected to clean up state and terminate.
* HUP - Often a reset request, esp if a daemon can reread config files and adjust while running.
* QUIT - Similar to TERM but it defaults to producing a core dump if not caught.

## Kill
Kill can kill a process, or send any signal. It defaults to TERM signal. This means that it cannot guarantee to kill a process as TERM can be caught and ignored.

	#Kill with TERM default (if no signal defined)
	$ kill [-signal] pid
	
	#Kill with KILL signal
	$ kill -9 pid #Guaranteed to kill a process - 9 is KILL
	 
	# Kill by name in Linux such as Ubuntu. On UNIX, it might kill all user processes. If user root, shuts computer down.
	$ killall name
	
**pgrep** and **pkill* commands for Linux search for processes by nam.

	$ sudo pkill -u ben #Kill all of Ben's processes with TERM
	
## Process States

* Runnable - Can be executed
* Sleeping - Waiting on some resource
* Zombie - Trying to die
* Stopped - Suspended (Not allowed to execute)



	



