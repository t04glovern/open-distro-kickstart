# Open Distro Elasticsearch Kickstart

A short kickstart repo for working with Open Distro Elasticsearch in a summarised way. Based heavily on the work on the [offical site](https://opendistro.github.io/for-elasticsearch-docs/docs/install/docker/)

## Cluster

### Cluster Up

```bash
docker-compose up -d
```

Kibana should now be available on [http://localhost:5601](http://localhost:5601)

***NOTE:** If your cluster fails to start you might need to increaase the max virtual memory using the following*

```bash
sudo sysctl -w vm.max_map_count=262144
```

***NOTE:** If you are on Windows, will need to increase the memory allocated to Docker to at least 4G.*

### Cluster Down

```bash
docker-compose down -v
```

## perf-top

If URL for the binary doesn't work, get the latest versions from [https://opendistro.github.io/for-elasticsearch/downloads.html](https://opendistro.github.io/for-elasticsearch/downloads.html)

### MacOS

```bash
wget https://d3g5vo6xdbdb9a.cloudfront.net/downloads/perftop/perf-top-1.8.0.0-MACOS.zip && \
    unzip perf-top-1.8.0.0-MACOS.zip && \
    rm perf-top-1.8.0.0-MACOS.zip

# Options
./perf-top-macos --dashboard ClusterNetworkMemoryAnalysis   --endpoint localhost:9600
./perf-top-macos --dashboard ClusterOverview                --endpoint localhost:9600
./perf-top-macos --dashboard ClusterThreadAnalysis          --endpoint localhost:9600
./perf-top-macos --dashboard NodeAnalysis                   --endpoint localhost:9600
```

### Linux

```bash
wget https://d3g5vo6xdbdb9a.cloudfront.net/downloads/perftop/perf-top-1.8.0.0-LINUX.zip && \
    unzip perf-top-1.8.0.0-LINUX.zip && \
    rm perf-top-1.8.0.0-LINUX.zip

# Options
# You might not need to include `TERM=xterm`
TERM=xterm ./perf-top-linux --dashboard ClusterNetworkMemoryAnalysis   --endpoint localhost:9600
TERM=xterm ./perf-top-linux --dashboard ClusterOverview                --endpoint localhost:9600
TERM=xterm ./perf-top-linux --dashboard ClusterThreadAnalysis          --endpoint localhost:9600
TERM=xterm ./perf-top-linux --dashboard NodeAnalysis                   --endpoint localhost:9600
```

## Data Ingestion

You need [elasticdump](https://github.com/taskrabbit/elasticsearch-dump) to load in the data

```bash
NODE_TLS_REJECT_UNAUTHORIZED=0 elasticdump \
    --input=es_dollop/dollop_episode_mapping.json \
    --output=https://admin:admin@localhost:9200/episodes \
    --type=mapping

NODE_TLS_REJECT_UNAUTHORIZED=0 elasticdump \
    --input=es_dollop/dollop_episode.json \
    --output=https://admin:admin@localhost:9200/episodes \
    --type=data

NODE_TLS_REJECT_UNAUTHORIZED=0 elasticdump \
    --input=es_dollop/dollop_paragraph_mapping.json \
    --output=https://admin:admin@localhost:9200/paragraphs \
    --type=mapping

NODE_TLS_REJECT_UNAUTHORIZED=0 elasticdump \
    --input=es_dollop/dollop_paragraph.json \
    --output=https://admin:admin@localhost:9200/paragraphs \
    --type=data
```

Load in the dashboard from [es_dollop/dashboard.json](es_dollop/dashboard.json)

![Dollop Example](img/dollop-example.png)
