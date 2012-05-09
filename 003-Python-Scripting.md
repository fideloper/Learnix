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

	#!/usr/bin/python















