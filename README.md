Datbase Server
============

Install a new database. Using [PostgreSQL](http://www.postgresql.org/) 9.2.2 and [PostGIS] (http://postgis.net/) 2.1.0

Instructions (installation)
---------------------------


<br />
Instructions for setting up Postgres 9.2.2 and PostGIS 2.1.0 on Ubuntu 12.04 (EC2)
This version installs everything from the source packages includeing all the dependices on a fresh EC2.

----------------------------------------------------------------------------------
### Install 'make', g++, python, etc
	sudo apt-get update
	sudo apt-get -y upgrade 
	sudo apt-get install -y make libxml2-dev libxslt-dev python-software-properties build-essential zlib1g-dev libreadline-dev libgdal1-dev 
	mkdir /home/ubuntu/installs
	cd /home/ubuntu/installs	
	sudo wget http://ftp.postgresql.org/pub/source/v9.2.2/postgresql-9.2.2.tar.bz2
	bzip2 -d postgresql-9.2.2.tar.bz2
	tar -xf postgresql-9.2.2.tar
	cd postgresql-9.2.2
	./configure --with-libxml --with-libxslt
	make
	sudo make install
	
	## postgis dependency installs
	cd /home/ubuntu/installs/postgresql-9.2.2/contrib/fuzzystrmatch
	make
	sudo make install
	#
	cd /home/ubuntu/installs
	mkdir postgis
	cd postgis
	#
	wget http://download.osgeo.org/proj/proj-4.8.0.tar.gz
	gzip -d proj-4.8.0.tar.gz
	tar -xvf proj-4.8.0.tar
	cd proj-4.8.0
	./configure
	make
	sudo make install
	#
	cd /home/ubuntu/installs/postgis
	wget http://download.osgeo.org/geos/geos-3.4.0.tar.bz2
	bzip2 -d geos-3.4.0.tar.bz2
	tar -xvf geos-3.4.0.tar
	cd geos-3.4.0
	./configure
	make
	sudo make install
	#
	cd /home/ubuntu/installs/postgis
	wget http://oss.metaparadigm.com/json-c/json-c-0.9.tar.gz
	gzip -d json-c-0.9.tar.gz
	tar -xvf json-c-0.9.tar
	cd json-c-0.9
	./configure
	make
	sudo make install
	#
	cd /home/ubuntu/installs/postgis
	wget http://download.osgeo.org/gdal/gdal-1.9.2.tar.gz
	gzip -d gdal-1.9.2.tar.gz
	tar -xvf gdal-1.9.2.tar
	cd gdal-1.9.2
	./configure
	make
	sudo make install
	#
	cd /home/ubuntu/installs/postgis
	wget http://postgis.net/stuff/postgis-2.1.0.tar.gz
	gzip -d postgis-2.1.0.tar.gz
	tar -xf postgis-2.1.0.tar
	cd postgis-2.1.0
	./configure --with-pgconfig=/usr/local/pgsql/bin/pg_config --with-raster
	make
	sudo make install
	#
	cd extensions/postgis
	make clean
	make
	sudo make install
	#
	cd ..
	cd postgis_topology
	make clean
	make
	sudo make install

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





