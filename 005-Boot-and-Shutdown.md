# Boot Process

### Ubuntu startup
Ubuntu start up scripts uses Upstart, which is event driven. It will start/stop services based on detected hardware and hardware changes.

`/etc/event.d` contains a collection of jobs (Job Definition Files). This includes rc scripts for various run levels. See corresponding `/etc/rc2.d/rc` files.

To maintain compatibilities, admins should use **updates-rc.d** command.

	$ update-rc.d *service* { start | stop } *sequence* *runlevels* .
	# Sequence: Order in which startup script should be run
	# Run Level: What run level it should run as
	# Terminate parsing with a period

Services that start late in run-level should end early when system exists that level.

**Examples (CUPS is a print server):**

	$ sudo update-rc.d cups start 80 2 3 4 5 . stop 20 S 1 6 .
	# Adds "start" instances as sequence 80 in run levels 2,3,4,5 and "stop" @ sequence 20 un run levels S,1,6

Default run levels are editable in `/etc/event.d/rc-default`.

### Shutting Down
Using **shutdown** command is absolute best. You can tell it to shutdown after a time has elapsed, giving logged-in users a message a bout upcoming shutdowns. You can tailer this message with a reason / planned uptime. Users can't log in during this period, but will see the message if attempting to.

Shutting down effects harddrive mounting, etc. **fsck** is often called, but can be skipped.

`/sbin/shutdown`: Time, -r (Reboot), -h (halt), -f(skip fsk, ubuntu does by default?)

	$ sudo shutdown -h 09:30 "Going down for scheduled maintenance." 
	$ sudo shutdown -h +15 "Going down in 15 minutes."
	
**Halt** performs essential shutdown duties. Logs shutdown, kills non-essential processes and executes the **sync** system call, waits for filesystem writes to complete an then halts the kernel.
**Halt -n** precents the **sync** call. 
**Reboots** is almost idential to **halt** but it causes the machine to restart instead of halting.

	$ sudo shutdown -P now #Powers off immeidiately after halt

