# mac-docker-log-monitoring

```
docker run -d --network mac-net-docker -p 8080:8080 mac-docker

docker run -d -p 9200:9200 --network mac-net-docker -e "discovery.type=single-node" --name elasticsearch docker.elastic.co/elasticsearch/elasticsearch:6.6.2

docker run -d -p 5601:5601 --network mac-net-docker --name kibana docker.elastic.co/kibana/kibana:6.6.2

docker run -d -v $(pwd)/logstash/:/usr/share/logstash/pipeline/ --network mac-net-docker --name logstash docker.elastic.co/logstash/logstash:6.6.2
 
docker run -d \
  --name=filebeat \
  --user=root \
  --volume="$(pwd)/filebeat/filebeat.yml:/usr/share/filebeat/filebeat.yml:ro" \
  --volume="/var/lib/docker/containers:/var/lib/docker/containers:ro" \
  --volume="/var/run/docker.sock:/var/run/docker.sock:ro" \
  --network mac-net-docker \
  docker.elastic.co/beats/filebeat:6.6.2 filebeat -e -strict.perms=false 
```