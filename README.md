#### influxdb

docker run --name influxdb \
  -p 8083:8083 -p 8086:8086 -p 25826:25826/udp \
  -v /home/ala/backup:/usr/share/ \
  influxdb:1.6.4

docker exec -it influxdb bash
influx 
CREATE USER "admin" WITH PASSWORD 'admin' WITH ALL PRIVILEGES;
auth

CREATE DATABASE prometheus;

CREATE USER "prom" with password 'prom';

GRANT ALL ON prometheus TO prom;

ALTER RETENTION POLICY "autogen" ON "prometheus" DURATION 1d REPLICATION 1 SHARD DURATION 1d DEFAULT;

SHOW RETENTION POLICIES ON prometheus;
exit
influxd restore -metadir /var/lib/influxdb/meta -database prometheus -datadir /var/lib/influxdb/data /usr/share/
exit

sudo docker stop influxdb
sudo docker start influxdb

#### grafana
docker run --name grafana \
  -p 3000:3000 \
  --link influxdb \
  grafana/grafana:5.1.0
  
  ###### juypter

docker run -it -p 8888:8888 -p 4040:4040 -v ~:/home/jovyan/workspace jupyter/all-spark-notebook
