## Access Control

Processes decide how to use ownership and groups, so no exact method. Filesystem, vs certain system proceses vs kernel process can behave differently. Only filesystem takes groups into account for access control, for instance.

####Filesystem
File owners can restrict read,write and execute on files they own. They can also set a group. File owners can also decide what a group, or "group owner" can do to a file.

Groups are edited in `/etc/group` but are also commonly controlled by NIS or LDAP servers in shared environments.

	$ ls -l
	# Lists files, shows permissions as well as owner and group owner.

Users and Groups have number ID's (UID and GID) found in `/etc/passwd` and `/etc/group` respectively.

####Root Users
The root *superuser* has a UID of 0. You can technically changes it's name, but don't. Root users can do any valid operation.

**Setuid** and **setgid** allow you to set an executable file's permissions. This allows non-priveledged users to run commands that would normally take priviledges (Such as changing their own password with **passwd**, which uses `/etc/shadow`).

There are still shortcomings to these approaches in the real world. Some more modern access control architectures attempt to help in that regard.

####Role-based access control
RBAC, very similar to ACL in programming - Roles are created and assign permissions to users, rather than permissions being assigned directly to users. Roles support inheritance of other roles.

SELinux / POSIX - usually used for regulated security constraints. Simpler ACL's are often used in day-to-day for processes such as printing which everyone might need.

####PAM: Pluggable Authnentiction Modules
PAM is for authentication (Is this really user X?). PAM is a wrapper for multiple authentication libraries. A program may use PAM instead of coding their own authentication, simply choosign with style of authentication to use.

####Kerberos
Kerberos is a style of authentication, most often used with **PAM**. Kerberos often runs as a third party (Separate server) which checks authentication.

### Real World

#### Root Password
Make a pass-phrase. "Shocking nonsense" - Culturally shocking but jibberish phrase, more than 8 characters in length.

Better idea to disallow remote login as root, as user data is collected (History of commands, etc).

Ubuntu has no root password set - You never log in as root. You DO use **sudo**.

##### su: Substitute User Identity
Slightly better than root is **su**. **su** prompts for the root password and then starts up a root shell, which continues until you hit ctr+d or the command **exit**.

You can **su** into other accounts other than root. Useful for debugging. Some systems need you to be part of member group 'wheel' to use **su**.

	$ su - username
	#Promoted for users password. Dash option makes su spawn a shell in login mode.
	#Better to use /bin/us or /usr/bin/su (direct path) to makesure no hackers sneak another 'su' into your PATH to harvest passwords

##### sudo: limited su
**Sudo** takes a command as an argument, and runs it as root (or other priviledged user). It uses the file `/etc/sudoers` which lists people who are authorized to use **sudo** and allowed commands on that host. If a command is allowed, it prompts the user for their ***own*** password (Not root password). The password prompt works for ~5 minutes before asking again (Configurable).

**Sudo** keeps a log of the command, who ran it, the directory is was run from and the time invoked.

Edit the `/etc/sudoers` files using **visudo**, which checks to make sure no one else is editing the file, invokes and editor and then verifies syntax.

**Sudo** lets you create an ACL system without the fanciness. You can split root permissions up among groups and people.


##### Sudo with non-root users
There are system users that often are priviledged and defined by the system. Typically any user with uid under 100. UIDs under 10 are often system accounts, while between 10 and 100 are pseudo-users associated with specific software.

You can/should replace encrypted password fields of these special users in `etc/shadow` with a star (*) so they can't be logged into. Set their shells to `/bin/false` or `/bin/nologin` as well to protect agaist exploits or use of SSH keys (passwordless login).



