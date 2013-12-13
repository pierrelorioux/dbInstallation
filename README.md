Database Server
============

Install a new database. Using [PostgreSQL](http://www.postgresql.org/) 9.3.0 and [PostGIS] (http://postgis.net/) 2.1.0

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


	sudo su postgres
	
	psql
	
	alter user postgres with password '<your password here>';
	
	\q
	exit
	
	--or VI or VIM or whatever editor
	sudo pico /etc/postgresql/9.3/main/postgresql.conf
	
	--from this 
	#listen_addresses = ‘localhost’  
	--to 
	listen_addresses = ‘*’  
	
	sudo pico /etc/postgresql/9.3/main/pg_hba.conf
	--add this line:
	host    all             all             0.0.0.0/0               md5
	
	sudo /etc/init.d/postgresql restart

<br />
You should be good to go….





