


## POST request to deezium endpoint giving the json file having connectino to postgres db
=====================================================================================
curl -i -X POST -H "Accept:application/json" -H "Content-Type:application/json" 127.0.0.1:8083/connectors/ --data "debezium.json"



## Command to run 
docker run --tty \
--network postgrs_debezium_cdc_default \
confluent/cp-kafkacat \
kafkacat -b kafka:9092 -C \
-s key=s -s value =avro \
-r http://schema-registry:8081 \
-t postgres.public.student
