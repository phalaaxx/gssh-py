gssh
----

Run ssh command on a group of servers simultaneously.
This project was inspired from *mpssh* and is written in pure Python. Its intention is to replace gssh-go and outline differences when writing the same project in both Python and Golang.


Requirements
------------

In order to use gssh, the ssh binary from openssh package must be installed in user's path.
Also the machine running gssh should be able to connect to every server listed in the file with hosts without a password - either with a passwordless key or with ssh agent. 


Usage
-----

A list of servers is mandatory to use gssh. The list is a plain text file with one server at a line (no username):

	cat << EOF > servers.txt
	server1.domain.tld
	server2.domain2.tld
	1.2.3.4
	EOF


To actually run a command on all files from the list:

	./gssh --file servers.txt 'uptime'
	or
	./gssh -f servers.txt 'uptime'


Alternative method to run gssh is to supply list of servers to standard input:

	cat << EOF | gssh 'uptime'
	server1.domain.tld
	server2.domain2.tld
	1.2.3.4
	EOF
	

Or to cat list files:

	cat servers.txt servers2.txt | gssh 'uname -r'


A full list of currently supported arguments can be obtained with the -h option:

	gssh -h
	usage: gssh [-h] [-d DELAY] [-f FILE] [-p PROCS] [-n] [-u USER] command
	
	positional arguments:
	  command               the actual command to run on servers
	
	optional arguments:
	  -h, --help            show this help message and exit
	  -d DELAY, --delay DELAY
	                        delay between each ssh fork (default 100 msec)
	  -f FILE, --file FILE  file with the list of hosts (default: read from stdin)
	  -p PROCS, --procs PROCS
	                        number of parallel ssh processes (default: 500)
	  -n, --nostrict        disable strict ssh fingerprint checking
	  -u USER, --user USER  ssh login as this username