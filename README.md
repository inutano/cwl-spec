# CWL-metrics

[![DOI](https://zenodo.org/badge/130311460.svg)](https://zenodo.org/badge/latestdoi/130311460)

CWL-metrics is a framework to collect and analyze computational resource usage of workflow runs based on the [Common Workflow Language (CWL)](https://www.commonwl.org). CWL-metrics launches a daemon process to catch `cwltool` processes, [Telegraf](https://github.com/influxdata/telegraf) to collect the resource usage via docker API, and [Elasticsearch](https://github.com/elastic/elasticsearch) to store the collected data in.

Visit [github pages](https://inutano.github.io/cwl-metrics/) for more details.

## Prerequisites

- `git`
- `curl`
- `perl`
- [`docker`](https://www.google.com/search?q=install+docker)
- [`docker-compose`](https://docs.docker.com/compose/install/) (version 3)
- [`cwltool`](https://github.com/common-workflow-language/cwltool)
  - recommended version: `1.0.20180820141117` or later

## Install

Use `curl` to fetch the install script from GitHub and exec via bash command as:

```
$ curl "https://raw.githubusercontent.com/inutano/cwl-metrics/master/bin/cwl-metrics" | bash
```

This will do followings:

- Check prerequisites
- Create `$HOME/.cwlmetrics`
- Fetch required tools
  - CWL-metrics daemon script (this repository)
  - [docker-metrics-collector](https://github.com/inutano/docker-metrics-collector) repository
  - [cwl-log-generator](https://github.com/inutano/cwl-log-generator) docker container
  - [cwl-metrics-client](https://github.com/inutano/cwl-metrics-client) docker container
- Generate config files
- Run CWL-metrics


## Troubleshooting

### CWL-metrics starting process stopped with showing "Creating Elasticsearch index..."

This happens because Elasticsearch did not launch successfully because of the machine setting on `vm.max_map_count`. Follow the guide below to set it to 262144.

```
# Linux
$ sudo sysctl -w vm.max_map_count=262144
$ grep vm.max_map_count /etc/sysctl.conf
vm.max_map_count=262144

# On mac os
$ screen ~/Library/Containers/com.docker.docker/Data/com.docker.driver.amd64-linux/tty
$ sudo sysctl -w vm.max_map_count=262144

# Windows and macOS with Docker Toolbox
$ docker-machine ssh
$ sudo sysctl -w vm.max_map_count=262144
```

If you already set and still have a problem on launching CWL-metrics, please let us know by creating an [issue](https://github.com/inutano/cwl-metrics/issues).
