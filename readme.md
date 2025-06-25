
# Postgres Change Data Captures (PG-CDC)



## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Installation](#installation)
- [Configuration](#configuration)
- [Testing](#testing)
- [Problems](#problems)
- [Contact](#contact)
- [References](#references)

---

## Overview

This script captures PostgreSQL WAL files and automates change data capture (CDC) using Debezium connector. It leverages a database connector and Kafka to stream and process database changes efficiently.

## Features

- Easy integration with PostgreSQL databases
- Real-time change data capture
- Scalable and efficient event streaming
- Customizable processing pipelines
- Robust error handling and recovery

## Installation

```bash
# Clone the repository
git clone https://github.com/BMLande/pg-cdc.git
cd pg-cdc

```

# Install dependencies

```bash
$ npm install
```

# run conatiners
    
    - kafka
    - postgres
    - zookeeper
    - debezium connector

Hit below to cmd to run containers for above images

`$ docker-compose up --build`

When all images running successfully, you can star kafka UI on browser with

`http://localhost:8080/`

Also, you can start kafka topic subscriber script , with

`$ node index.js`

## Configuration

When all images running successfully, you need to create postgreas coonection with debzium provided connected, which basically setup replicationn configuration, Hit below API to perform this.

Http Type : POST

`url : http://localhost:8083/connectors`

``` json : body
{
	"name": "postgres-connector",
	"config": {
		"connector.class": "io.debezium.connector.postgresql.PostgresConnector",
		"database.hostname": "postgres",
		"database.port": "5432",
		"database.user": "postgres",
		"database.password": "postgres",
		"database.dbname": "testdb",
		"database.server.name": "testdb-server",
		"plugin.name": "pgoutput",
		"slot.name": "debezium_slot_2",
		"publication.name": "debezium_pub_3",
		"table.include.list": "public.users",
		"tombstones.on.delete": "false",
		"key.converter.schemas.enable": "false",
		"value.converter.schemas.enable": "false",
		"topic.prefix": "testdb-server",
		"snapshot.mode": "always"
	}
}
```


## Testing

Now everything is setup, you can test PG cdc by inserting records , with

- connect to DB
`docker exec -it $(docker ps --filter "ancestor=postgres:15-alpine" --format "{{.ID}}") psql -U postgres -d testdb`

- insert Record
`INSERT INTO public.users (id, name, email) VALUES (321, 'Test User', 'te1st@exaspdsafddaddle.com');`

Afetr, you can
- visit kafka UI and check message on added topic
- visit nodejs script running termianl and can see the received message to subscriber


## Problems

- if message not arriving on topic try to run sql cmds from init.sql manually
- delete the coonctor config reload and then create new one


## Contact

Bhagvat Lande – [@BhagvatL](https://twitter.com/BhagvatL) 

Project Link: [https://github.com/BMLande/pg-cdc](https://github.com/BMLande/pg-cdc)

## References
- https://www.postgresql.org/docs/9.4/logicaldecoding-explanation.html
- https://debezium.io/documentation/reference/3.1/connectors/postgresql.html#postgresql-in-the-cloud
- https://www.youtube.com/watch?v=YNfQon8sC9w&t=39s





