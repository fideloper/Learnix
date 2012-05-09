## Python Scripting

Note use of whitespace, rather than curly brackets, parenthesis, etc. Also note the "shebang"

	#!/usr/bin/python
	
	import sys
	
	a = sys.argv[1]
	
	if a === "1":
		print 'a is one'
		print 'This is still the then clause'
	else:
		print 'a is', a
		print 'This is still the else clause'
	
	print 'This is after the if statement'

You can write code on multile lines by escaping newline character with a blackslash. In that case, only the first line of indentation is required.

	#!/usr/bin/python	f = open('/etc/passwd', 'r')	print f.readline(),	print f.readline(),	f.close()
	#YIELDS:	at:x:25:25:Batch jobs daemon:/var/spool/atjobs:/bin/true	bin:x:1:1:bin:/bin:/bin/true
In the above, the , after readline() suppresses newline character, as they already exist in the file.
	#!/usr/bin/python	import sys	import os	
	def show_usage(message, code = 1):		print message		print "%s: source_dir dest_dir" % sys.argv[0] #$0, the path used to call this file		sys.exit(code) #1=error
	if len(sys.argv) != 3: #$0, $1, $2		show_usage("2 arguments required; you supplied %d" % (len(sys.argv) - 1))	elif not os.path.isdir(sys.argv[1]):		show_usage("Invalid source directory")	elif not os.path.isdir(sys.argv[2]):		show_usage("Invalid destination directory")	
	source, dest = sys.argv[1:3]	print "Source Directory is", source	print "Destination Directory is", dest
For loops have power. Note the use of multiple variables being uncompacted ($k=>$v). \1 is getting patter matches.
	#!/usr/bin/python	import sys	import re	suits = { 'Bashful':'red', 'Sneezy':'green', 'Doc':'blue', 'Dopey':'orange', 'Grumpy':'yellow', 'Happy':'taupe', 'Sleepy':'puce' }	pattern = re.compile("(%s)" % sys.argv[1])	
	for dwarf, color in suits.items():		if pattern.search(dwarf) or pattern.search(color):			print "%s's dwarf suit is %s." % \			(pattern.sub(r"_\1_", dwarf), pattern.sub(r"_\1_", color))			break		else:			print "No dwarves or dwarf suits matched the pattern."
Usage:
	$ python dwarfsearch '[aeiou]{2}'	Sn_ee_zy's dwarf suit is gr_ee_n.	
	$ python dwarfsearch go	No dwarves or dwarf suits matched the pattern.
	
Notes: 
1. If a system call fails, include the perror string ($! in Perl).
