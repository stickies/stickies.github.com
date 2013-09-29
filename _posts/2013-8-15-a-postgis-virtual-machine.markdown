---
layout: stickie
title: Postgis Up and Running with Vagrant
meta: postgresql virtualbox ruby
---
[Postgis](http://postgis.net/) is without doubt an awesome technology. If you're into relational databases and geo then you'll love it if you don't already.

So I wanted to get it up and running on my Mac ASAP, to play around and maybe build something with it, but unfortunately I already have a version of Postgres running, with lots of work related material in the db, that I don't really want to mess up in my attempts to spatially enable the database! If you haven't seen or used the venerable [Virtual Box](https://www.virtualbox.org/), then you're missing out, you will need it for this solution to work.

Once you have virtual box installed, you need to install the vagrant gem. This is another awesome tool that acts as a wrapper around VirtualBox.

### The process

    gem install vagrant

    mkdir postgis-box

    cd postgis-box

    vagrant init precise32 http://files.vagrantup.com/precise32.box

Create a file called bootstrap.sh ...

    #!/usr/bin/env bash

    ### postgis 2.0

      apt-get update --fix-missing

      apt-get -y install build-essential
      apt-get -y install postgresql-9.1
      apt-get -y install postgresql-server-dev-9.1
      apt-get -y install postgres-client-common
      apt-get -y install postgresql-contrib-9.1 pgadmin3
      apt-get -y install libxml2-dev
      apt-get -y install proj libjson0-dev xsltproc docbook-xsl docbook-mathml gettext·


      apt-get -y install python-software-properties
      apt-add-repository ppa:olivier-berten/geo
      apt-get update
      apt-get -y install libgdal-dev

      apt-get -y install g++ ruby ruby1.8-dev swig swig2.0··
      wget http://download.osgeo.org/geos/geos-3.3.3.tar.bz2
      tar xvfj geos-3.3.3.tar.bz2
      cd geos-3.3.3
      ./configure --enable-ruby --prefix=/usr
      make
      make install
      cd ..

      wget http://postgis.refractions.net/download/postgis-2.0.0.tar.gz
      tar xfvz postgis-2.0.0.tar.gz
      cd postgis-2.0.0
      ./configure
      make
      make install
      ldconfig
      make comments-install
      cp loader/shp2pgsql /usr/local/bin/
      cd ..

      sudo -su postgres createuser vagrant -s
      sudo -su vagrant createdb
      sudo -u postgres createdb template_postgis
      sudo -u postgres psql -d template_postgis -f /usr/share/postgresql/9.1/contrib/postgis-2.0/postgis.sql
      sudo -u postgres psql -d template_postgis -f /usr/share/postgresql/9.1/contrib/postgis-2.0/postgis_comments.sql
      sudo -u postgres psql -d template_postgis -f /usr/share/postgresql/9.1/contrib/postgis-2.0/rtpostgis.sql
      sudo -u postgres psql -d template_postgis -f /usr/share/postgresql/9.1/contrib/postgis-2.0/raster_comments.sql
      sudo -u postgres psql -d template_postgis -f /usr/share/postgresql/9.1/contrib/postgis-2.0/topology.sql
      sudo -u postgres psql -d template_postgis -f /usr/share/postgresql/9.1/contrib/postgis-2.0/topology_comments.sql

    ### END postgis 2.0

Then add this line to the Vagrantfile that vagrant created when you initialized the box.

    config.vm.provision :shell, :path => "bootstrap.sh"

Now run

    vagrant up

Et voila.

