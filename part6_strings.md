String and quotes
=================

There are 2 ways to use strings in bash:
* `'hello world'`
* `"hello world"`
	
Unlike some programing languages which don't mind which kind of quotations you use, bash does to some degree.
This becomes important whe nyou need to add variables as part of strings, lets demonstrate this with an example:

	echo 'my path is: $PATH'
	echo "my path is: $PATH"
	
The single quotes will echo the literal string $PATH, whereas the double quotes will evaluate it as a variable.
This is also the case when handling arguements (`$1` etc).

Using braces
------------
Sometimes it can be useful to include braces in variable expansions, for example:

	time=23
	echo "your current record is $times"
	
We want to add a `s` to the end of the time variable (indicating seconds), however bash tries to find the variable `times`.
to tell bash that the `s` isa literal string and not part of the variable name we do it using braces:

	time=23
    echo "your current record is ${time}s"

arguement strings
-----------------
passing an arguement to a command is done simple by succeeding the command with the arguements themselves separated with spaces.
for example if we wish to delete multiple files we can do this as follows:

	rm file1.txt file2.txt 
	
Now suppose one of our arguements (in this case the filenames) have a space in them:

	rm file1.txt file2.txt 'final file.txt'
	
Notice how we have to quote the entire arguement, otherwise bash wil interpret the space between `final` and `file.txt` as syntax and not literal.
We can alternatively use backslashes to escape the space:
	
	rm file1.txt file2.txt final\ file.txt
	
But I personally think this is less readable. Having several of thsese can make the command cryptic.
Quotes are always better than no quotes, if things get messy, always quote your data. 
*If there is a space or symbol in your data, you must quote it. And always double quote any variable or arguement expansion*.

Lets demonstrate the importance of double quoting variable expansions:
	
	read -p 'what document file would you like to remove? ' filename
	
	rm -r home/miloudbelarbi/Documents/$filename
	
If the `IFS` (internal field separator) is not set to bash's default new line character and instead set to nothing, and you enter ` file1` (that is space and then file1),
the `rm` command will remove all files not just file1, becuase it might expand `$filename` as follows: `rm -r home/miloudbelarbi/Documents/ file1`.
Notice the space before `file1`, to eliminate this nasty bug we should wrap our variable expansions in double quotes:

	rm -r "home/miloudbelarbi/Documents/$filename"
	
Yes you could write and if statement to check the IFS, but why not just use double quotes? :)