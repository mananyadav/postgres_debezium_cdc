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
curl -i -X POST -H "Accept:application/json" -H "Content-Type:application/json" 127.0.0.1:8083/connectors/ --data "@debezium.json"

<img width="1253" alt="image" src="https://user-images.githubusercontent.com/72122903/204280713-11664898-968e-4109-85ae-95d81049927e.png">


## Command to start the consumer inside the kafka docker to view the msg inserted/updaeted/deleted in postgresql
docker exec -it acb52c35b431a22419251d1fc3f15dbab29c6987402390f72fa9ec8570e16951 \
kafka-console-consumer --bootstrap-server kafka:9092 \
--topic postgres.public.student \
--from-beginning
  
## Command to run  - tail the kafka topic to see if any change is coming in the database table 
docker run --tty \
--network postgrs_debezium_cdc_default \
confluent/cp-kafkacat \
kafkacat -b kafka:9092 -C \
-s key=s -s value =avro \
-r http://schema-registry:8081 \
-t postgres.public.student
