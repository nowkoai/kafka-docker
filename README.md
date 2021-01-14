# Docker-ComposeでKafkaクラスタ構築
docker-compose up -d
docker-compose ps

# クライアント/コンテナにログイン
docker exec -it cli /bin/bash

# トピックの作成（クライアントからKafkaクラスタに接続して作成）
kafka-topics --bootstrap-server broker:9092 --create --topic sample-topic --partitions 3 replication-factor 1
kafka-topics --bootstrap-server broker:9092 --list
kafka-topics --bootstrap-server broker:9092 --describe --topic sample-topic

# Producerの作成（クライアント#1でデータ送信）
kafka-console-producer --broker-list broker:9092 --topic sample-topic

# Consumerの作成（クライアント#2でデータ送信）
docker exec -it cli /bin/bash
kafka-console-consumer --bootstrap-server broker:9092 --topic sample-topic --group G1 --from-beginning
kafka-consumer-groups --bootstrap-server broker:9092 --list

# Consumer Groupによる負荷分散確認
docker exec -it cli /bin/bash
kafka-consumer-groups --bootstrap-server broker:9092 --describe --group G1
kafka-console-consumer --bootstrap-server broker:9092 --topic sample-topic --group G1 --from-beginning