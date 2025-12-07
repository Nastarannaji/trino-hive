Trino & Hive Data Warehouse Project
overview:

This project demonstrates the setup of a big-data environment with Hive (Docker) and Trino (local installation).
The goal was to integrate Hive with Trino, load sample TPCH data, and compare query performance between the two engines.

Key skills demonstrated:

Docker & containerized Hadoop/Hive setup

Local Trino installation and configuration

Big-data integration (Hive ↔ Trino)

Benchmarking and performance analysis	


1. Setting up Hive (Docker)

Hive and Hadoop services are deployed using Docker Compose:

```
docker-compose up -d
```

This starts the following containers:

namenode (HDFS)

datanode (HDFS)

hive-metastore-postgresql

hive-metastore

hive-server 

Access Hive:

	docker exec -it hive-server bash
	beeline -u jdbc:hive://localhost:10000


You can now create schemas, tables, and load TPCH data.


2. Installing Trino Locally

Trino was installed directly on the host machine following the official documentation: 

https://trino.io/docs/current/installation/deployment.html

Steps:

Download and extract the Trino server tarball:

https://github.com/trinodb/trino/releases/download/478/trino-server-478.tar.gz

	
Download Trino CLI (trino-cli-476-executable.jar) and rename it:

	mv trino-cli-476-executable.jar trino.jar
	chmod +x trino.jar


Start Trino server:

	sudo /usr/lib/trino-server-476/bin/launcher run


Connect using the CLI:

	java -jar trino.jar --server http://localhost:8080

Add Hostnames (If Needed)

To ensure local Trino can resolve Hadoop nodes:

	sudo nano /etc/hosts


Add:

	172.20.0.2 namenode
	172.20.0.3 datanode
	...

3. Connecting Trino to Hive

To allow Trino to access Hive tables, a catalog configuration is required:

Create the file:

	/usr/lib/trino-server-476/etc/catalog/hive.properties


Add the following content:

	connector.name=hive
	hive.metastore.uri=thrift://<hive-metastore-ip>:9083


hive refers to the HiveServer connector, which Trino uses to communicate with Hive.
Replace <hive-metastore-ip> with the container IP from Docker network.

Verify the connection in Trino CLI:

	SHOW CATALOGS;
	SHOW SCHEMAS FROM hive;
	SHOW TABLES FROM hive.tpch_data;

4. Running TPCH Benchmark Queries

Loaded TPCH SF1 data into Hive.

Ran 15 standard TPCH queries in both Hive and Trino.

Compared query execution times to evaluate performance.

Key results:

Trino executed queries 10–60x faster than Hive in complex join and aggregation queries.

Trino demonstrated better memory efficiency and scalability.

5. Project Highlights

Designed and deployed a full big-data workflow using Docker and local tools.

Integrated Hive with Trino for real-time querying.

Benchmarked and compared query performance between engines.

Gained hands-on experience with:

Docker, Hadoop, Hive, PostgreSQL

Trino configuration and CLI

Catalog configuration and data integration
