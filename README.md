# kafka_leader_exchange
This project will give the step wise process to see how the kafka change the leadership when any broker goes down.

@@@@@@@@@@@!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!  Step 1 !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!@@@@@@@@@@@@@@@@@@@


docker-compose up -d --scale=kafka=3


@@@@@@@@@@@!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!  Step 2 !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!@@@@@@@@@@@@@@@@@@@


// Create a topic t1

docker exec -t kafka-docker_kafka_1 kafka-topics.sh --bootstrap-server :9092 --create --topic t1 --partitions 3 --replication-factor 2


@@@@@@@@@@@!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!  Step 3 !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!@@@@@@@@@@@@@@@@@@@


// Describe topic t1

docker exec -t kafka-docker_kafka_1 kafka-topics.sh --bootstrap-server :9092 --describe --topic t1


@@@@@@@@@@@!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!  Step 4 !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!@@@@@@@@@@@@@@@@@@@


docker stop kafka-docker_kafka_2


@@@@@@@@@@@!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!  Step 5 !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!@@@@@@@@@@@@@@@@@@@


// Describe topic t1

docker exec -t kafka-docker_kafka_1 kafka-topics.sh --bootstrap-server :9092 --describe --topic t1


@@@@@@@@@@@!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!  Step 6 !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!@@@@@@@@@@@@@@@@@@@


docker start kafka-docker_kafka_2


@@@@@@@@@@@!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!  Step 7 !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!@@@@@@@@@@@@@@@@@@@


// Describe topic t1

docker exec -t kafka-docker_kafka_1 kafka-topics.sh --bootstrap-server :9092 --describe --topic t1


@@@@@@@@@@@!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!  Step 8 !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!@@@@@@@@@@@@@@@@@@@


// Change the election type

docker exec -t kafka-docker_kafka_1 kafka-leader-election.sh --bootstrap-server :9092 --topic t1 --partition 0 --election-type preferred


@@@@@@@@@@@!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!  Step 9 !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!@@@@@@@@@@@@@@@@@@@


// Describe topic t1

docker exec -t kafka-docker_kafka_1 kafka-topics.sh --bootstrap-server :9092 --describe --topic t1


@@@@@@@@@@@!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!  Final Clean Up !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!@@@@@@@@@@@@@@@@@@@


// Type a message in producer window to see the message printed out in consumer's Observe the logs of the running docker-compose up (with no -d)

// Or use docker logs -f kafka-docker_kafka_1

// Ctrl-C to shut down all the processes

// You may also consider removing the containers so you always start afresh

docker-compose rm
