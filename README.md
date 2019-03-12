# Open Distro Elasticsearch Kickstart

A short kickstart repo for working with Open Distro Elasticsearch in a summarised way. Based heavily on the work on the [offical site](https://opendistro.github.io/for-elasticsearch-docs/docs/install/docker/)

## Cluster

### Cluster Up

```bash
docker-compose up -d
```

Kibana should now be available on [http://localhost:5601](http://localhost:5601)

### Cluster Down

```bash
docker-compose down -v
```

## perf-top

If URL for binary doesnt work, get the latest versions from [https://opendistro.github.io/for-elasticsearch/downloads.html](https://opendistro.github.io/for-elasticsearch/downloads.html)

### MacOS

```bash
wget https://d3g5vo6xdbdb9a.cloudfront.net/downloads/perftop/perf-top-0.7.0.0-MACOS.zip && \
    unzip perf-top-0.7.0.0-MACOS.zip && \
    rm perf-top-0.7.0.0-MACOS.zip

# Options
./perf-top-macos --dashboard dashboards/ClusterNetworkMemoryAnalysis.json   --endpoint localhost:9600
./perf-top-macos --dashboard dashboards/ClusterOverview.json                --endpoint localhost:9600
./perf-top-macos --dashboard dashboards/ClusterThreadAnalysis.json          --endpoint localhost:9600
./perf-top-macos --dashboard dashboards/NodeAnalysis.json                   --endpoint localhost:9600
```

### Linux

```bash
wget https://d3g5vo6xdbdb9a.cloudfront.net/downloads/perftop/perf-top-0.7.0.0-LINUX.zip && \
    unzip perf-top-0.7.0.0-LINUX.zip && \
    rm perf-top-0.7.0.0-LINUX.zip

# Options
./perf-top-linux --dashboard dashboards/ClusterNetworkMemoryAnalysis.json   --endpoint localhost:9600
./perf-top-linux --dashboard dashboards/ClusterOverview.json                --endpoint localhost:9600
./perf-top-linux --dashboard dashboards/ClusterThreadAnalysis.json          --endpoint localhost:9600
./perf-top-linux --dashboard dashboards/NodeAnalysis.json                   --endpoint localhost:9600
```

## Data Ingestion

You need [elasticdump](https://github.com/taskrabbit/elasticsearch-dump) to load in the data

```bash
elasticdump \
    --input=es_dollop/dollop_episode_mapping.json \
    --output=http://admin:admin@localhost:9200/episodes --insecure \
    --type=mapping

elasticdump \
    --input=es_dollop/dollop_episode.json \
    --output=http://admin:admin@localhost:9200/episodes --insecure \
    --type=data

elasticdump \
    --input=es_dollop/dollop_paragraph_mapping.json \
    --output=http://admin:admin@localhost:9200/paragraphs --insecure \
    --type=mapping

elasticdump \
    --input=es_dollop/dollop_paragraph.json \
    --output=http://admin:admin@localhost:9200/paragraphs --insecure \
    --type=data
```

Load in the dashboard from [es_dollop/dashboard.json](es_dollop/dashboard.json)

![Dollop Example](img/dollop-example.png)
