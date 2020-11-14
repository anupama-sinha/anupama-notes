### ELK(Elastic, Logstash & Kibana)
* Open source products by Elastic company
* Allows to take data from any source, in any format & to search, analyze & visualize data in realtime
* Centralized logging for any number of servers & applications

### ELK Stack Architecture
* Logs : Server/application logs
* Logstash : Shipping, processing and storing logs. Collects logs, events data. Parses & transforms data
* Elastic/Elastic Search : Stores logs/transformed data from Logstash(Store, Search & Indexing). NoSQL DB based on lucene search engine & build on Restful APIs
* Kibana : Web interface visualization tool hosted through Nginx or Apache. Uses Elastic search DB to explore, visualize & share
* Beats : Data collection. Hence, ELK became ELK Stack
* Messaging Queue can be used for large data logs and maintaining resiliency

### Working
* Logs -> Beats -> Message Queue Buffering -> Logstash -> Elastic Search -> Kibana

### Alternatives
* Splunk : Commercial tool providing on-prem & cloud solution. Quite accurate and fast

### ELK Use Cases
* Netflix's Security Log
* LinkedIn's performance & security
* Medium's production issues & DynamoDB Hotstop tracking

### Best Practices
* Logs written to single ELK instance

### Installation
#### Type 1 : Local Download [ELK](https://www.elastic.co/downloads/)
* Start Elastic search server(C:\elasticsearch\bin\elasticsearch.bat) & check if running in http://localhost:9200/
* Start Kibana(C:\kibana\bin\kibana.bat) & check if running in http://localhost:5601/
* Start Logstash(C:\logstash\bin) and type 
> cmd binlogstash -e 'input { stdin { } } output { stdout {} }'
#### Type 2 : Running ELK in Docker
* In Docker Quickstart Termainal, run below commands to download & run ELK
1. Elastic search
> docker pull docker.elastic.co/elasticsearch/elasticsearch:7.9.3
* Start single node cluster
> docker run -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.9.3


### References
* https://www.edureka.co/blog/elk-stack-tutorial/
* https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html