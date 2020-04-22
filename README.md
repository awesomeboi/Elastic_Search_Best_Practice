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

We will use Kibana to run Elastic command, analysis and visualization.

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

## STEP 3 Install Logstash

We will use Logstash to upload and manipulate datas to Elastic search.

**Download Link :** https://www.elastic.co/downloads/logstash

After downloading Logstash, unzip it.

![GitHub Logo](https://www.elastic.co/guide/en/logstash/7.6/static/images/basic_logstash_pipeline.png)

Logstash is a bit different from other tools. It has an input and output. Also It has a filter part that you can do manipulations on data.

To run Logstash properly we need to prepare conf file. In this conf file we need to define input port, how to filter the data and output port.

We can use Logstash to directly read data, but for this pratice we will use Filebeat to read data. For now we jump to Filebeat part then we come back to Logstash and create a conf file.

## STEP 4 Install FileBeat

FileBeat is a tool to collect log datas like .csv files.

**Download Link :** https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.2-darwin-x86_64.tar.gz

After downloading FileBeat, unzip it.

We need to manipulate filebeat.yml file on FileBeat folder.

Open it with Sublime or vim.

There is a input and output part on the file. In input part we need to define FileBeat where to find our data.

In output part, we need to define FileBeat where to send data.

In our example we read sample.csv data and send it to logstash.

For this we crete the conf file below.

```
filebeat:
  prospectors:
    -
      paths:
        - /tmp/data/*.csv
      exclude_files: ['sample.csv']
      encoding: utf-8
      input_type: log
      document_type: myindex
      tail_files: false
  registry_file: /var/lib/filebeat/registry
output:
  logstash:
    hosts: ["logstash:5044"]
output.console:
  pretty: true
```
Now go back to Logstash. We need to create a logstash.conf file to define where to listen as an input, how to filter data and where to send data.

For this practice we put these codes to logstash.conf file.
```

input {
  beats {
    port => 5044
    type => "mytype"
  }
}

filter {
  csv {
    separator => ","
    columns => ["Ref","ID","Case_Number","Date","Block","IUCR","Primary_Type","Description","Location_Description","Arrest","Domestic","Beat","District","Ward","Community_Area","FBI_Code","X_Coordinate","Y_Coordinate","Year","Updated_On","Latitude","Longitude","Location"]
    remove_field => ["Location"]
  }
}

output {
  stdout
  {

  }
  
  elasticsearch
  {
    hosts => ["elasticsearch:9200"]
    index => "myindex"
    template_name => "myindex"
    template => "/etc/logstash/conf.d/template.json"
    template_overwrite => true
    document_type => "mytype"
    pipeline => "my_pipeline"
  }
}


      tail_files: false
