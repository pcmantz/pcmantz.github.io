---
title: "Building DBD::Pg for Perlbrew on Ubuntu"
date: 2012-05-23T00:00:00-06:00
slug: "building-dbd-pg-for-perlbrew-on-ubuntu"

---

I get a lot of benefit from managing my own Perl via perlbrew, but I'm fine using Ubuntu's packaged Postgres sever. However, Ubuntu strips down a lot of its applications into components, and you have to install development versions and header files separately if you want to compile everything. I'm putting this here for anyone who might be trying to install DBD::Pg on top of perlbrew. I'm not going to claim this will work in all circumstances, and it assumes you have already installed Postgres, but it definitely worked for me.

    sudo apt-get install libpq5 postgresql-server-dev-9.1
    POSTGRES_HOME='/usr/lib/postgresql/9.1' POSTGRES_INCLUDE='/usr/include/postgresql' cpanm Bundle::DBD::Pg
