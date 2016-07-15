Less QQ, more Pew Pew!
======================

`qq` is a utility to aid in the discovery and execution of well-behaved
commands in well-defined locations.

Activate `qq` by putting a command called `qq-foo` in your `$PATH` which prints
out sub-commands and their location on disk.

```
	$ which qq-foo
	~/bin/qq-foo

	$ ~/bin/qq-foo
	These commands are related to "foo" operationts
	bar ~/bin/bar
	baz ~/bin/baz

	$ qq
	Commands available
	  foo - These commands are related to "foo" operationts
	  qq - commands related to qq itself

	$ qq foo
	Commands available
	  bar - This is the bar command
	  baz - These commands are related to "foo" operationts

	$ ~/bin/baz --help
	This is the baz command
	  --help        print this text
	  --make-money  literally prints money

	$ qq foo bar
	Error: need to pass --make-money option to make money
	$ qq foo bar --make-money
	money money money!
```

Alternatively `qq` will also recursively traverse up the directory tree looking
for sub-directories called `bin` that also contain a file called `README` or
`README.*`.  Commands in that directory will be linked in at the top level and 
the first line of the `README` file will be printed as the header.

```
	$ cd ~/Git/my-project/src/foo/bar

	$ wc -l ~/Git/my-project/bin/README.txt
	10

	$ head -1 ~/Git/my-project/bin/README.txt
	My Project Ad-Hoc Scripts

	$ ls -1 ~/Git/my-project/bin/
	do_something
	another_thing

	$ qq
	Commands available
	  qq - commands related to qq itself
	My Project Ad-Hoc Scripts
	  do_something - The do_something command, this most definitely does something
	  another_thing - This command does something else, but different

	$ ~/Git/my-project/bin/do_something --help
	The do_something command, this most definitely does something
	  --help        print this text
	  --really      need confirmation before we do something

	$ qq do_something --really
	Please wait, really doing something...
	Done!

	$ cd ~ && qq
	Commands available
	  qq - commands related to qq itself
```

Commands that `qq` attempts to execute MUST BE SAFE TO DO SO WITH `--help` AS
A PARAMETER (but that's already the case, right?).

Commands will only be executed if they are exposed via `qq-*` injection, or
if they live in a directory called `bin` along with a `README` in that same
directory (that is how you opt-in to `qq` discovery).  The first line of the
`README` is used as the description for the commands in that directory.

The better your ad-hoc scripts are, the less useful `qq` becomes.  Think of
`qq` as the carrot to entice people to write better ad-hoc scripts so you
don't have to hit them with a clue bat.

