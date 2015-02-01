rollerbackup
============

This Ant script aims to automate backup Apache Roller instance running on PostgreSQL and Linux. the script backups one database and one data directory.

Before use, edit variables in build-props.xml to suit your environment and requirement.

Prerequisites
-------------

- Requires JRE and Apache Ant (Tested with version 1.9.4).
- PostgreSQL database. need to modify the backup command if you use other database such as MySQL or Derby.
- keyfile has no passphrase.
- Public key is already added to authorized_keys on the server.
- The server is already registered in known_hosts.

Main targets
------------

These targets are intended to use regularly.

- backup
	1. Dump the database to temporary file on the server
	2. Download and delete the database dump file
	3. Create tar archive of data directory to temporary file on the server
	4. Download and delete the tar archive
- purge
	1. Purge older backups that aged than ${prop.preserves}. see build-props.xml for detail

NOTE: backup target needs free space which is almost the same to the amount of data because it creates backup archive on the server.

Crontab example
---------------

My crontab example which runs purge and backup at every morning (means every AM 05:02) as follows:

	ANT_HOME=/Users/kyle/apache-ant-1.9.4
	JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_31.jdk/Contents/Home
	2 5 * * * $ANT_HOME/bin/ant -f /Users/kyle/rollerbackup/build.xml purge backup 2>&1 | logger -p local1.notice

How backup destination will be
------------------------------

	$ tree
	.
	└── backup
		├── 20150131_210254
		│   ├── roller.dump
		│   └── rollerdata.tar
		├── 20150131_211138
		│   ├── roller.dump
		│   └── rollerdata.tar
		└── 20150131_211939
			├── roller.dump
			└── rollerdata.tar
