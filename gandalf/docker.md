## How can I use it in my docker environment?

Gandalf has the following dependencies:
- Pelipper: the system used to send user notifications
- Postgres: the database where the used will be saved. It needs the plugin `uuid-ossp` installed

Gandalf also need a SMTP server due to pelipper requires it. So in order to include gandalf in your docker environment please copy the following code:

```yml
mailhog:
  image: mailhog/mailhog
  container_name: mailhog
  ports: 
    - 1025:1025
    - 8025:8025

pelipper:
  image: ghcr.io/antartical/pelipper
  container_name: pelipper
  ports:
    - "9000:9000"
  environment:
    - SMTP_HOST=mailhog
    - SMTP_PORT=1025
    - SMTP_USER=admin
    - SMTP_PASSWORD=admin

postgres:
  image: postgres:13.1-alpine
  container_name: postgres
  restart: always
  environment:
    - POSTGRES_USER=root
    - POSTGRES_PASSWORD=root
    - POSTGRES_DB=frodo
    - POSTGRES_EXTENSIONS=uuid-ossp
  ports:
    - "5432:5432"
  volumes:
    - <your_script_path>:/docker-entrypoint-initdb.d
    - antartical.frodo:/var/lib/postgresql/data

gandalf:
  image: ghcr.io/antartical/gandalf
  container_name: gandalf
  ports:
    - "9100:9100"
  environment:
    - ENVIRONMENT=docker
    - JWT_TOKEN_TTL=60
    - JWT_TOKEN_RTTL=1440
    - JWT_TOKEN_KEY=mysupersecret
    - PELIPPER_HOST=http://pelipper:9000
    - PELIPPER_SMTP_ACCOUNT=accounts@antartical.com
    - POSTGRES_USER=root
    - POSTGRES_PASSWORD=root
    - POSTGRES_DB=frodo
    - POSTGRES_DB_TEST=test
    - POSTGRES_HOST=postgres
    - POSTGRES_PORT=5432
```

Please, notice that you need to include the following script in your postgres configuration `<your_script_path>:/docker-entrypoint-initdb.d` in order to install `uuid-ossp` automatically:

```bash
#!/bin/bash

set -e
set -u

function create_user_and_database() {
	local database=$1
	echo "  Creating user and database '$database'"
	psql -v ON_ERROR_STOP=1 --username "$POSTGRES_USER" <<-EOSQL
	    CREATE USER $database;
	    CREATE DATABASE $database;
	    GRANT ALL PRIVILEGES ON DATABASE $database TO $database;
		
EOSQL
}

function install_extensions() {
	local databse=$1
	for extension in $(echo $POSTGRES_EXTENSIONS | tr ',' ' '); do
		psql -v ON_ERROR_STOP=1 --username "$POSTGRES_USER" -d "$db" <<-EOSQL
			create extension if not exists "$extension";
		
EOSQL
	done
}

if [ -n "$POSTGRES_MULTIPLE_DATABASES" ]; then
	echo "Multiple database creation requested: $POSTGRES_MULTIPLE_DATABASES"
	for db in $(echo $POSTGRES_MULTIPLE_DATABASES | tr ',' ' '); do
		create_user_and_database $db
		install_extensions $db
	done
	echo "Multiple databases created"
fi
```

This script also support multiple database creation in the case you do not want to define another db for your system.
