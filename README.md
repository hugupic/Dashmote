# Data Engineering Pipeline
## Run as a Docker
Take ubuntu 18.04 as example
```console
$ apt-get update
$ apt-get install docker-compose
$ git clone https://github.com/hugupic/mote-data-analyse.git
$ cd mote-data-analyse
$ docker-compose up
```
To run it as a daemon,
```console
$ docker-compose up -d
```
The default port in `docker-compose.yml` is `5000`. Modifying `5000:5000` to `80:5000` can have the server listening on port `80`.

### Transform the JSON files to MySQL database
```console
$ python transform.py
```
### Run the server
```
$ python api.py
```

## RESTful API server
The server has been deployed at 127.0.0.1

### `GET` outlets who sell certain brands
Following returns a list of outlets that has coca inside its brand name.
```console
$ curl 127.0.0.1/outlets/brand/contains/coca
```

### `GET` a list of outlets
```console
$ curl 127.0.0.1/outlets/source/ubereats
```
 
### `GET` menu items above a certain price
```console
$ curl 127.0.0.1/menus/price/above/40
```

### `POST` a new outlet
```console
$ curl 127.0.0.1/outlets -d "id_outlet=1029912390213" -d "name=hello_world" -d "source=ubereats" -d "address=unknown" -d "country=CN" -d "phone=+852 123 1234" -d "reviews_nr=123" -X POST -v
```

## Unittest
### Configure the test database
Create a MySQL database locally
```console
$ mkdir ~/data && mkdir ~/data/mote && mkdir ~/data/mote/mysql
$ docker run --name dashmote -v ~/data/mote/mysql:/var/lib/mysql -p 3306:3306 -p 33060:33060 -e MYSQL_ROOT_PASSWORD="toor" --restart=always -d mysql:latest
```

Enter the MySQL database and create the test database
```console
$ docker exec -it dashmote /bin/bash
$ mysql -u root -p # enter the password here
$ CREATE DATABASE dashmote_test;
$ exit
```

### Run the unittests
pass