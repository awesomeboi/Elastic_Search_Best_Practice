# Elastic_Search_Best_Practice

This repo is about making a Elastic Stack on a local machine. I will explain everything for MacOS. If you want to implement for different machine you can find alternative links on official website. 

Elastic Stack is a stack includes every important that you will need when you want to use ElasticSearch.

Official Document of Elastic Stack : https://www.elastic.co/guide/en/elastic-stack/current/overview.html

This repo will explain every step to make an Elastic Stack. Also there will be a demo to upload data to server and use some ElasticSearch commands.

## STEP 1 Install Elastic Search

The core part of Elastic Stack is ElasticSearch. We need to download ElasticSearch and run server on local machine.

**Download Link :** https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.6.2-darwin-x86_64.tar.gz

(for alternative links : https://www.elastic.co/guide/en/elasticsearch/reference/7.6/targz.html)

After downloading ElasticSearch, unzip it.

Open Terminal.

Go to ElasticSearch folder with cd command on Terminal.

Run command below.

```
./bin/elasticsearch
```
To check if ElasticSearch is online, run the code below.

```
curl -X GET "localhost:9200/?pretty"
```
There should be a response like below.
```
{
  "name" : "Cp8oag6",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "AT69_T_DTp-1qgIJlatQqA",
  "version" : {
    "number" : "7.6.2",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "f27399d",
    "build_date" : "2016-03-30T09:51:41.449Z",
    "build_snapshot" : false,
    "lucene_version" : "8.4.0",
    "minimum_wire_compatibility_version" : "1.2.3",
    "minimum_index_compatibility_version" : "1.2.3"
  },
  "tagline" : "You Know, for Search"
}
```
## STEP 2 Install Kibana

We will use kibana to run Elastic command, analysis and visualization.

**Download Link :** https://artifacts.elastic.co/downloads/kibana/kibana-7.6.2-darwin-x86_64.tar.gz

(for alternative links : https://www.elastic.co/guide/en/kibana/7.6/targz.html#install-darwin64)

**Important Note :** Alternatively, you can download Kibana and/or, which contains only features that are available under the Apache 2.0 license. You can run regular version ElasticSearch with oss version Kibana and visa versa. They both need to be same version.

After downloading Kibana, unzip it.

Open a new Terminal. Do not close ElasticSearch Terminal window.

Go to Kibana folder with cd command on Terminal.

Run command below.

```
./bin/kibana
```
To check if Kibana is online, opene browser.

Write go to 127.0.0.1 address.

If you a see Kibana home page Kibana is online.




