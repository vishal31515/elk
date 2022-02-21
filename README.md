# ELK-with-filebeat-and-Docker

ELK with Filebeat and Docker - Simple, Open Source &amp; Easy way to file logging.

## Work flow
- So, what is the ELK Stack? "ELK" is the acronym for three open source projects: Elasticsearch, Logstash, and Kibana. Elasticsearch is a search and analytics engine. Logstash is a server‑side data processing pipeline that ingests data from multiple sources simultaneously, transforms it, and then sends it to a "stash" like Elasticsearch. Kibana lets users visualize data with charts and graphs in Elasticsearch.

- Logs: Server logs that need to be analyzed are identified.
- Logstash: Collect logs and events data. It even parses and transforms data and send to elasticserach.
- ElasticSearch: The transformed data from Logstash is Store, Search, and indexed.
- Kibana: Kibana uses Elasticsearch DB to Explore, Visualize, and Share
- mylog -> filebeat -> logstash -> elasticsearch <- kibana


## System Cofigurations
- Minimum 8 GB RAM required.

## Usage

### ELK with Filebeat Extension
1. Docker & Docker-compose install (Ubuntu)

2. Git clone this repository
```
git clone https://github.com/vishal31515/elk.git
```

3. Set to making your log file into 'mylog' folder and change log index template.
- Filebeat can auto-detect to *.log file was made, updated and push it to logstash.
- Change your log index file of "logstash.template.json".

4. Docker-compose up command
```
docker-compose up --build
```
- Elasticsearch runs on [http://localhost:9200]
- Logstah runs on [http://localhost:5044]
- Kibana service runs on [http://localhost:5601]


### Customize Config
- logstash.template.json : Change it to your log index
```
# Make your own log index
{
    ...
    "mappings": {
        "properties": {
            "name": {
                "type": "keyword"
            },
            "class": {
                "type": "keyword"
            },
            "state": {
                "type": "integer"
            },
            "@timestamp": {
                "type": "date"
            }
        }
    }
}
```
- logstash.conf
```
# Change 'timestamp' to your log custom timestamp key
filter {
  ...
  date{
    match => ["timestamp", "UNIX_MS"]
    target => "@timestamp"
  }
}
```
```
# Change 'time.localtime' to your location time
filter {
  ...
  ruby {
    code => "event.set('indexDay', event.get('[@timestamp]').time.localtime('+09:00').strftime('%Y%m%d'))"
  }
}
```

- [More about ELK stack](https://www.elastic.co/what-is/elk-stack)
