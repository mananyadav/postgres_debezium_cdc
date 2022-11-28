## Execute docker compose 
docker compose up -d 

## Enter postgres cli 
docker exec -it 4ad799907b2e202f0b311ce21ebadb5c18128d499a00a20f5e88c60789c7b41f /bin/sh

## login to postgressql 
psql -U docker -d exampledb -W
<Enter passowrd> : docker

## create a table 
CREATE TABLE student (id integer primary key , name varchar);

SELECT * FROm student;

ALTER TABLE public.student REPLICA IDENTITY FULL


## SetUp a debezium connector 
Tell for which table we have to setup the debezium connector. check the debeziu.json config file 
POST request to deezium endpoint giving the json file having connectino to postgres db
command below using POST request: 
*curl -i -X POST -H "Accept:application/json" -H "Content-Type:application/json" 127.0.0.1:8083/connectors/ --data "debezium.json"*



## Command to run 
docker run --tty \
--network postgrs_debezium_cdc_default \
confluent/cp-kafkacat \
kafkacat -b kafka:9092 -C \
-s key=s -s value =avro \
-r http://schema-registry:8081 \
-t postgres.public.student
