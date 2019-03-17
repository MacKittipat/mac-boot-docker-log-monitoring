# mac-docker-log-monitoring

```
docker build -t mac-docker .

docker network create mac-net-docker

docker run -d --log-driver gelf --log-opt gelf-address=udp://$(docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' logstash):12201 --network mac-net-docker -p 8080:8080 mac-docker

docker run -d -p 9200:9200 --network mac-net-docker -e "discovery.type=single-node" --name elasticsearch docker.elastic.co/elasticsearch/elasticsearch:6.6.2

docker run -d -p 5601:5601 --network mac-net-docker --name kibana docker.elastic.co/kibana/kibana:6.6.2

docker run -d -v ~/projects/mac-docker-log-monitoring/logstash-pipeline/:/usr/share/logstash/pipeline/ --network mac-net-docker --name logstash docker.elastic.co/logstash/logstash:6.6.2
 
```