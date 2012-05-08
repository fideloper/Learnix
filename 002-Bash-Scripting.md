## Bash Scripting
Anything in command line goes into bash scripts, and vise versa.

	$ chmod -x myfile.sh #Make executable. Extension doesn't matter.
	$ bash myfile.sh #New instance of Bash
	$ ./myfile.sh #Use current terminal session. Useful for environmental variables that are only in this session
	
	$ ./myfile.sh
	#is the same as:
	$ source myfile.sh
	
Find .log files in /var/log, ignore .do-not-touch directory, and get iterate over results. This just prints out the command we'd want to use (mv, to rename file).
	
	$ find /var/log -type f -name '*.log' | grep -v .do-not-touch | while read fname	> do	> echo mv $fname ${fname/.log/.LOG/}	> done
Lets make those mv commands run. Pipe to bash directly! -x will print each command before running it. -fc will let you specify a file to write it to, so you can repeate the commands any time! (Generate code!)
	$ find . -type f -name '*.log ' | grep -v .do-not-touch | while read fname; do	echo mv $fname ${fname/.log/.LOG/}; done | bash -x
	
### printf > echo
printf accepts better formatting (And I think escaping?)

	$ echo "\taa\tbb\tcc\n"	#Yields: \taa\tbb\tcc\n	$ printf "\taa\tbb\tcc\n"	#Yields: aa bb cc
	
### Get user input
#!/bin/bash	
	#!/bin/bash	echo -n "Enter your name: "	read user_name
		if [ -n "$user_name" ]; then		echo "Hello $user_name!"		exit 0	else		echo "You did not tell me your name!"		exit 1	fi
		# Usage: $ sh readexample
**Command line arguments (CLA)**
1. $1 gets first CLA, $2 gets second2. $0 gets name by which command was run (could be anything, its filepath)
3. $# gets number of CLAs
4. $* gets all arguments at once (None of these include $0)

Example:


	#!/bin/bash
	function show_usage {		echo "Usage: $0 source_dir dest_dir"		exit 1	}	# Main program starts here	if [ $# -ne 2 ]; then		show_usage	else # There are two arguments		if [ -d $1 ]; then			source_dir=$1		else				echo 'Invalid source directory'			show_usage		fi		if [ -d $2 ]; then			dest_dir=$2		else			echo 'Invalid destination directory'			show_usage		fi	fi	printf "Source directory is ${source_dir}\n"	printf "Destination directory is ${dest_dir}\n"
*Note 
1. that this doesn't write to STDERR (It should)
2. Functions take arguments in same way ($1, $2). They behave like shell files themselves. You can define functions in .bash_profile instead of aliases, in fact. Mo' Powa!
3. If statements - if, elif, else, fi. `if [test (-eq, -ne)] && [other_test] then…`

More Operations:

	x = y x -eq y x is equal to y	x != y x -ne y x is not equal to y	x < y x -lt y x is less than y	x <= y x -le y x is less than or equal to y	x > y x -gt y x is greater than y	x >= y x -ge y x is greater than or equal to y	-n x – x is not null	-z x – x is null
Also, excitingly,
	-d file file exists and is a directory	-e file file exists	-f file file exists and is a regular file	-r file You have read permission on file	-s file file exists and is not empty	-w file You have write permission on file	file1 -nt file2 file1 is newer than file2	file1 -ot file2 file1 is older than file2	
*Note: Consider case statements instead of if,elif,else

Check out for…in loop, AND how it iterates over file names!

	#!/bin/bash	suffix=BACKUP--`date +%Y%m%d-%H%M`	for script in *.sh; do		newname=”$script.$suffix”		echo "Copying $script to $newname..."		cp $script $newname	done
1. For..do…done loops
2. While something; do … done

**Consider:**
	
	#!/bin/bash	exec 0<$1 #Read STDIN from file which is first CLA. If not file, get error	counter=1 	while read line; do  #while loop can handle commands!		echo "$counter: $line"		$((counter++)) #$(()) forces numeric evaluation	done
	
**But really, use Python.**