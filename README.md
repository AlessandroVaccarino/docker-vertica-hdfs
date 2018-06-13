# HP Vertica with HDFS Docker container

## What is HP Vertica with HDFS Docker container project?
A complete installation of [HP Vertica Community Edition](https://www.vertica.com/) wih HDFS, to enable [integration with Apache Spark](https://my.vertica.com/blog/integrating-apache-spark/), into a Docker container. Just run and enjoy.
It's a derived project from the standalone [HP Vertica Docker container](https://github.com/AlessandroVaccarino/docker-vertica) project

### Run the container

To run your first Vertica container, just type:

`docker run -p 5433:5433 -p 5450:5450 -p 9000:9000 -p 50070:50070 -p 50075:50075 -p 50090:50090 -p 50105:50105 -it -d alessandrov87/docker-vertica-hdfs /etc/bootstrap.sh`

If you want to store DB data outside the container, map data path under your desired local folder. Eg:
`docker run -p 5433:5433 -p 5450:5450 -p 9000:9000 -p 50070:50070 -p 50075:50075 -p 50090:50090 -p 50105:50105 -v $(pwd)/data:/srv/vertica/db -it -d alessandrov87/docker-vertica-hdfs /etc/bootstrap.sh`

When boots up, Vertica checks if DB and/or data are available:
- If DB is available, but there are no data under */srv/vertica/db,* DB is removed and another empty DB is created
- If DB is available and there are data under */srv/vertica/db*, the DB is started
- If DB is not available, and there are no data under */srv/vertica/db*, a new empty DB is created
- If DB is not available, but there are data uner */srv/vertica/db*, data are removed and an empty DB is created (I'm working on a "restore procedure", to attach data under a new DB, feel free to contribute)

It will download the latest version of HP Vertica HDFS Docker image available, create a container, start HP Vertica, start HDFS and expose ports.
So, after executed your container, go to `https://localhost:5450/`: you wil reach HP Vertica Management Console.
You can also reach the following ports:
- 5433, to access Vertica via Client (vsql, JDBC, ODBC,...)
- 50070, for HDFS Namenode Web UI
- 50075, for HDFS Datanode Web UI
- 50090, for Secondary HDFS Namenode Web UI
- 50105, for Backup/Checkpoint node
- 9000, for Namenode metadata service

To learn how to use HP Vertica, please refer to [HP Vertica Official Documentation](https://my.vertica.com/hpe-vertica-idol-documentation/) page.
For Vertica-Spark integration, please refer to [Integrating with Apache Spark](https://my.vertica.com/blog/integrating-apache-spark/) page.
