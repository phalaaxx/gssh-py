gssh
----

Run ssh command on a group of servers simultaneously.
This project was inspired from *mpssh* and is written in Python3. Its intention is to replace gssh-go and outline differences when writing the same project in both Python and Golang.


Requirements
------------

In order to use gssh, the ssh binary from openssh package must be installed in user's path.
Also the machine running gssh should be able to connect to every server listed in the file with hosts without a password - either with a passwordless key or with ssh agent. 


Usage
-----

A list of servers is mandatory to use gssh. The list is a plain text file with one server at a line (no username):

	$ cat << EOF > servers.txt
	server1.domain.tld
	server2.domain2.tld
	1.2.3.4
	EOF


To actually run a command on all files from the list:

	$ ./gssh --file servers.txt 'uptime'
	or
	$ ./gssh -f servers.txt 'uptime'


Alternative method to run gssh is to supply list of servers to standard input:

	$ cat << EOF | gssh 'uptime'
	server1.domain.tld
	server2.domain2.tld
	1.2.3.4
	EOF
	

Or to cat list files:

	$ cat servers.txt servers2.txt | gssh 'uname -r'

Another option is to read and parse ansible hosts file located in /etc/ansible/hosts.
This is supported with the option -a which tells gssh to read ansible hosts file.
In order to read servers only from a specific section in ansible hosts file, the -s option can be used.

    $ gssh -a -s webservers uptime


A full list of currently supported arguments can be obtained with the -h option:

    $ gssh -h
    usage: gssh [-h] [-d DELAY] [-f FILE] [-p PROCESSES] [-n] [-u USER] [-a]
                [-s SECTION]
                command
    
    positional arguments:
      command               the actual command to run on servers
    
    optional arguments:
      -h, --help            show this help message and exit
      -d DELAY, --delay DELAY
                            delay in milliseconds between each ssh fork (default
                            100)
      -f FILE, --file FILE  file with the list of hosts (default: read from stdin)
      -p PROCESSES, --processes PROCESSES
                            number of parallel ssh processes (default: 500)
      -n, --no-strict       disable strict ssh fingerprint checking
      -u USER, --user USER  ssh login as this username
      -a, --ansible         read ansible hosts file at /etc/ansible/hosts
      -s SECTION, --section SECTION
                            name of ansible ini section containing servers list

