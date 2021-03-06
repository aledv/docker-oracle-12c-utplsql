Oracle Standard Edition 12c Release 1 - utPLSQL framework
============================

Oracle Standard Edition 12c Release 1 on Ubuntu and utPLSQL framework
This **Dockerfile** is a [trusted build](https://hub.docker.com/r/aledv/docker-oracle-12c-utplsql/) of [Docker Registry](https://registry.hub.docker.com/).

### Installation

    docker pull aledv/docker-oracle-12c-utplsql

Run with 8080 and 1521 ports opened:

    docker run -d -p 8080:8080 -p 1521:1521 aledv/docker-oracle-12c-utplsql

Run and Install utPLSQL framework [http://utplsql.org/]

    docker run -d -p 8080:8080 -p 1521:1521 -e INSTALL_UTPLSQL=true aledv/docker-oracle-12c-utplsql

Run and Install utPLSQL framework [http://utplsql.org/] and Create User UT_USER

    docker run -d -p 8080:8080 -p 1521:1521 -e INSTALL_UTPLSQL=true -e CREATE_UT_USER=true aledv/docker-oracle-12c-utplsql

Run with data on host and reuse it:

    docker run -d -p 8080:8080 -p 1521:1521 -v /my/oracle/data:/u01/app/oracle aledv/docker-oracle-12c-utplsql

Run with Custom DBCA_TOTAL_MEMORY (in Mb):

    docker run -d -p 8080:8080 -p 1521:1521 -v /my/oracle/data:/u01/app/oracle -e DBCA_TOTAL_MEMORY=1024 aledv/docker-oracle-12c-utplsql
Connect database with following setting:

    hostname: localhost
    port: 1521
    sid: xe
    service name: xe
    username: system
    password: oracle

To connect using sqlplus:

<pre>
sqlplus system/oracle@//localhost:1521/xe
</pre>

Password for UT_USER (if exist):
    
    ut_user
    
Password for SYS & SYSTEM:

    oracle

Connect to Oracle Application Express web management console with following settings:

    http://localhost:8080/apex
    workspace: INTERNAL
    user: ADMIN
    password: 0Racle$

Apex upgrade up to v 5.*

    docker run -it --rm --volumes-from ${DB_CONTAINER_NAME} --link ${DB_CONTAINER_NAME}:oracle-database -e PASS=YourSYSPASS aledv/docker-oracle-12c-utplsql
Details could be found here: https://github.com/MaksymBilenko/docker-oracle-apex

Connect to Oracle Enterprise Management console with following settings:

    http://localhost:8080/em
    user: sys
    password: oracle
    connect as sysdba: true

By Default web management console is enabled. To disable add env variable:

    docker run -d -e WEB_CONSOLE=false -p 1521:1521 -v /my/oracle/data:/u01/app/oracle aledv/docker-oracle-12c-utplsql
    #You can Enable/Disable it on any time

Start with additional init scripts or dumps:

    docker run -d -p 1521:1521 -v /my/oracle/data:/u01/app/oracle -v /my/oracle/init/SCRIPTSorSQL:/docker-entrypoint-initdb.d aledv/docker-oracle-12c-utplsql
    
By default Import from `docker-entrypoint-initdb.d` is enabled only if you are initializing database (1st run).

To customize dump import use `IMPDP_OPTIONS` env variable like `-e IMPDP_OPTIONS="REMAP_TABLESPACE=FOO:BAR"`
To run import at any case add `-e IMPORT_FROM_VOLUME=true`

**In case of using DMP imports dump file should be named like ${IMPORT_SCHEME_NAME}.dmp**

**User credentials for imports are  ${IMPORT_SCHEME_NAME}/${IMPORT_SCHEME_NAME}**

If you have an issue with database init like DBCA operation failed, please reffer to this [issue](https://github.com/MaksymBilenko/docker-oracle-12c/issues/16)

**TODO LIST**

* Web management console HTTPS port
* Add functionality to run custom scripts on startup, for example User creation
* Add Parameter that would setup processes amount for database (Currently by default processes=300)
* Spike with clustering support
* Spike with DB migration from 11g

**In case of any issues please post it [here](https://github.com/MaksymBilenko/docker-oracle-12c/issues).**


