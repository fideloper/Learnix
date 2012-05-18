### Access Control

Processes decide how to use ownership and groups, so no exact method. Filesystem, vs certain system proceses vs kernel process can behave differently. Only filesystem takes groups into account for access control, for instance.

**Filesystem**
File owners can restrict read,write and execute on files they own. They can also set a group. File owners can also decide what a group, or "group owner" can do to a file.

Groups are edited in `/etc/group` but are also commonly controlled by NIS or LDAP servers in shared environments.

	$ ls -l
	# Lists files, shows permissions as well as owner and group owner.

Users and Groups have number ID's (UID and GID) found in `/etc/passwd` and `/etc/group` respectively.

**Root Users**
The root *superuser* has a UID of 0. You can technically changes it's name, but don't. Root users can do any valid operation.

**Setuid** and **setgid** allow you to set an executable file's permissions. This allows non-priveledged users to run commands that would normally take priviledges (Such as changing their own password with **passwd**, which uses `/etc/shadow`).