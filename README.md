Datbase Server
============

Install a new database. Using [PostgreSQL](http://www.postgresql.org/) 9.2.2 and [PostGIS] (http://postgis.net/) 2.1.0

Instructions (installation)
---------------------------


<br />
Instructions for setting up Postgres 9.3.0 and PostGIS 2.1.0 on Ubuntu 12.04 (EC2)
This version installs everything from the source packages includeing all the dependices on a fresh EC2.

----------------------------------------------------------------------------------

	sudo apt-get update
	sudo apt-get -y upgrade 
	
	sudo wget http://anonscm.debian.org/loggerhead/pkg-postgresql/postgresql-common/trunk/download/head:/apt.postgresql.org.s-20130224224205-px3qyst90b3xp8zj-1/apt.postgresql.org.sh
	
	sudo chmod 777 apt.postgresql.org.sh
	sudo ./apt.postgresql.org.sh
	(hit enter)
	
	sudo apt-get install postgresql-9.3 postgresql-contrib-9.3 postgresql-9.3-postgis-2.1 postgresql-9.3-postgis-scripts -y


	

<br />
Now configuration time

### Configure PostgreSQL database server
	##### Add 'postgres' user
		sudo adduser postgres
		Enter new UNIX password: 
		Retype new UNIX password:
	
	##### Make and set permissions on the pgsql directory
		sudo mkdir /usr/local/pgsql/data
		sudo chown postgres /usr/local/pgsql/data
	
	##### Initialize the postgres DB
		su - postgres
		/usr/local/pgsql/bin/initdb -D /usr/local/pgsql/data
	
	##### Start the service and logging
		/usr/local/pgsql/bin/postgres -D /usr/local/pgsql/data >logfile 2>&1 &
	
	##### Create a test DB
		/usr/local/pgsql/bin/createdb test
		/usr/local/pgsql/bin/psql test
		\q
		
	### Modify Files
		su - postgres
		cd /usr/local/pgsql/data
		pico postgresql.conf
		
		# remove the hash and change localhost to *, then save and exit pico
		listen_addresses = '*'

		pico pg_hba.conf
		# Add this line, then save and exit pico
		host  all all 0.0.0.0/0 md5
	
	### Config libraries
		sudo ldconfig 
	
	### Restart postgres service
		su - postgres
		/usr/local/pgsql/bin/postgres -D /usr/local/pgsql/data >logfile 2>&1 &
	
	
	##restart the server. 
	##log in and restart the postgres service.
    ##This will get the basic stuff up. 	

<br />
You should be good to go….





