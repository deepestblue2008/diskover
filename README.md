# diskover - File system crawler, disk space usage, file search engine and storage analytics powered by Elasticsearch

[![License](https://img.shields.io/github/license/shirosaidev/diskover.svg?label=License&maxAge=86400)](./LICENSE)
[![Release](https://img.shields.io/github/release/shirosaidev/diskover.svg?label=Release&maxAge=60)](https://github.com/shirosaidev/diskover/releases/latest)
[![Sponsor Patreon](https://img.shields.io/badge/Sponsor%20%24-Patreon-brightgreen.svg)](https://www.patreon.com/shirosaidev)
[![Donate PayPal](https://img.shields.io/badge/Donate%20%24-PayPal-brightgreen.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=CLF223XAS4W72)

<img align="left" width="249" height="189" src="docs/diskover.png?raw=true" hspace="5" vspace="5" alt="diskover">

diskover is an open source file system crawler and disk space usage software that uses [Elasticsearch](https://www.elastic.co) to index and manage data across heterogeneous storage systems. Using diskover, you are able to more effectively search and organize files and system administrators are able to manage storage infrastructure, efficiently provision storage, monitor and report on storage use, and effectively make decisions about new infrastructure purchases.

As the amount of file data generated by business' continues to expand, the stress on expensive storage infrastructure, users and system administrators, and IT budgets continues to grow.

Using diskover, users can identify old and unused files and give better insights into data change, file duplication and wasted space. diskover supports crawling local file-systems, over NFS/SMB, through TCP sockets with the Tree Walk Client or through http with the Storage Agent. Importing Amazon S3 inventory files is also supported. Plugin extensions can be used for adding additional meta data.

diskover is written and maintained by Chris Park (shirosai) and runs on Linux, OS X/macOS and Windows 10 (using windows subsystem for linux) using Python 2 or 3.

<blockquote><h3><q>This is the first tool I've found that can index 7m files/2m directories in under 20 min</q></h3> -- linuxserver.io community member</blockquote>

<div align="center"><img src="https://github.com/shirosaidev/diskover/blob/master/docs/diskover-diagram1.png?raw=true" alt="diskover diagram" width="800" height="525"/></div>

## Screenshots

diskover crawler and workerbots running in terminal<br>
<img align="left" width="400" src="https://github.com/shirosaidev/diskover/raw/master/docs/diskover-crawler-terminal-screenshot.png?raw=true" alt="diskover crawler">
<img width="400" src="https://github.com/shirosaidev/diskover/raw/master/docs/diskover-workerbot-terminal-screenshot.png?raw=true" alt="diskover worker bot"><br>
[diskover-web](https://github.com/shirosaidev/diskover-web) (diskover's web file manager, analytics app, file system search engine, rest-api)<br>
<img align="left" width="400" src="https://github.com/shirosaidev/diskover-web/raw/master/docs/diskover-web-dashboard-screenshot.png?raw=true" alt="diskover-web dashboard">
<img width="400" src="https://github.com/shirosaidev/diskover-web/raw/master/docs/diskover-web-filetree-screenshot.png?raw=true" alt="diskover-web file tree">
<img align="left" width="400" src="https://github.com/shirosaidev/diskover-web/raw/master/docs/diskover-web-advancedsearch-screenshot.png?raw=true" alt="diskover-web advanced search">
<img width="400" src="https://github.com/shirosaidev/diskover-web/raw/master/docs/diskover-web-tags-screenshot.png?raw=true" alt="diskover-web tags"><br>
Kibana dashboards/saved searches/visualizations and support for Gource<br>
<img align="left" width="400" src="docs/kibana-dashboarddark-screenshot.png?raw=true" alt="kibana-screenshot">
<img width="400" src="docs/diskover-gource1-screenshot.png?raw=true" alt="diskover-gource">

### diskover Gource videos

<a href="https://youtu.be/InlfK8GQ-kM"><img align="left" width="400" src="https://img.youtube.com/vi/InlfK8GQ-kM/0.jpg" alt="gource video"></a>
<a href="https://youtu.be/qKLJjZ0TMqA"><img width="400" src="https://img.youtube.com/vi/qKLJjZ0TMqA/0.jpg" alt="gource video"></a>

## Become a Patron & support shedding light on data darkness

If you are a fan of the project or you are using diskover and it's helping you save storage space, please consider supporting the project on [Patreon](https://www.patreon.com/shirosaidev) or [PayPal](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=CLF223XAS4W72). Thank you so much to all the fans and supporters!

## Installation Guide

### Requirements

* `Linux or OS X/macOS` (tested on OS X 10.11.6, Ubuntu 16.04/18.04), `Windows 10` (using Windows Subsystem for Linux)
* `Python 2.7. or Python 3.5./3.6.` (tested on Python 2.7.15, 3.6.5) **Python 3 recommended**
* `Python elasticsearch client module`
* `Python requests module`
* `Python scandir module`
* `Python progressbar2 module`
* `Python redis module`
* `Python rq module`
* `Elasticsearch 5.6.x` (local or [AWS ES Service](https://aws.amazon.com/elasticsearch-service/), tested on Elasticsearch 5.6.9) **Elasticsearch 6 not supported**
* `Redis 4.x` (tested on 4.0.9)

**See requirements.txt for specific python module version numbers since newer versions may not work with diskover.**

### Optional Installs

* [diskover-web](https://github.com/shirosaidev/diskover-web) (diskover's web file manager and analytics app)
* [storage agent](https://github.com/shirosaidev/diskover-storage-agent) (diskover's storage agent for running on remote storage)
* [tree walk client](https://github.com/shirosaidev/diskover-treewalk-client/) (diskover's tree walk client for running on remote storage)
* [saisoku](https://github.com/shirosaidev/saisoku) (data sync/mover between on-prem to cloud, etc)
* [sharesniffer](https://github.com/shirosaidev/sharesniffer) (for scanning your network for file shares and auto-mounting for crawls)
* [Redis RQ Dashboard](https://python-rq.org/docs/monitoring/) (for monitoring redis queue)
* [Kibana](https://www.elastic.co/products/kibana) (for visualizing Elasticsearch data, tested on Kibana 5.6.9)
* [X-Pack](https://www.elastic.co/downloads/x-pack) (Kibana plugin for graphs, reports, monitoring and http auth)
* [netdata](https://my-netdata.io/) (for realtime monitoring cpu/disk/mem/network/elasticsearch/redis/etc metrics, plugin for rq-dashboard in netdata directory)
* [Grafana ES dashboard](https://grafana.com/dashboards/878) (Grafana dashboard for Elasticsearch)
* [crontab-ui](https://github.com/alseambusher/crontab-ui) (web ui for managing cron jobs - for scheduling crawls)
* [cronkeep](https://github.com/cronkeep/cronkeep) (alternative web ui for managing cron jobs)
* [Gource](http://gource.io) (for Gource visualizations of diskover Elasticsearch data, see videos above)

### Download

```sh
$ git clone https://github.com/shirosaidev/diskover.git
$ cd diskover
```

[Download latest version](https://github.com/shirosaidev/diskover/releases/latest)


## Getting Started

Check Elasticsearch and Redis are running and are the required versions (see requirements above).
```sh
$ curl -X GET http://localhost:9200/
$ redis-cli info
```

Install Python dependencies using `pip`.

```sh
$ pip install -r requirements.txt
```

Copy diskover config `diskover.cfg.sample` to `diskover.cfg` and edit for your environment.

Start diskover worker bots (a good number might be cores x 2) with:

```sh
$ cd /path/with/diskover
$ python diskover_worker_bot.py
```

Worker bots can be added during a crawl to help with the queue. To run a worker bot in burst mode (quit after all jobs done), use the -b flag. If the queue is empty these bots will die, so use `rq info` or `rq-dashboard` to see if they are running. 

To start up multiple bots, run:

```sh
$ cd /path/with/diskover
$ ./diskover-bot-launcher.sh
``` 

By default, this will start up 8 bots. See -h for cli options including changing the number of bots to start. Bots can be run on the same host as the diskover.py crawler or multiple hosts in the network as long as they have the same nfs/cifs mountpoint as rootdir (-d path) and can connect to ES and Redis (see wiki for more info).

### Usage examples

Start diskover main job dispatcher and file tree crawler with (using adaptive batch size and optimize index cli flags):

```sh
$ python /path/to/diskover.py -d /rootpath/you/want/to/crawl -i diskover-indexname -a -O
```

**Defaults for crawl with no flags is to index from . (current directory) and files >0 Bytes and 0 days modified time. Empty files and directores are skipped (unless you use -s 0 and -e flags). Symlinks are not followed and skipped. Use -h to see cli options.**

Don't prompt user to overwrite existing index:

```sh
$ python /path/to/diskover.py -d /rootpath/you/want/to/crawl -i diskover-indexname -a -O -F
```

Crawl down to maximum tree depth of 3:

```sh
$ python diskover.py -i diskover-indexname -a -d /rootpath/to/crawl -M 3
```

Only index files which are >90 days modified time and >1 KB filesize:

```sh
$ python diskover.py -i diskover-indexname -a -d /rootpath/to/crawl -m +90 -s 1024
```

Only index files which have been modified in the last 7 days including empty files and dirs:

```sh
$ python diskover.py -i diskover-indexname -a -d /rootpath/to/crawl -m -7 -s 0 -e
```

Store cost per gb in es index from diskover.cfg settings and use size on disk (disk usage) instead of file size:

```sh
$ python diskover.py -i diskover-index -a -d /rootpath/to/crawl -G -S
```

Create index with just level 1 directories and files, then run background crawls in parallel for each directory in rootdir and merge the data into same index. After all crawls are finished, calculate rootdir doc's size/items counts:

```sh
$ python diskover.py -i diskover-indexname -a -d /rootpath/to/crawl --maxdepth 1
$ python diskover.py -i diskover-indexname -a -d /rootpath/to/crawl/dir1 --reindexrecurs -q &
$ python diskover.py -i diskover-indexname -a -d /rootpath/to/crawl/dir2 --reindexrecurs -q &
...
$ python diskover.py -i diskover-indexname -a -d /rootpath/to/crawl --dircalcsonly --maxdcdepth 0
```

## OVA image file (for vmware, etc)

Becoming a Patron gets you access to the OVA files running the latest version of diskover/diskover-web. Fastest way to get up and running diskover. Check out the [Patreon](https://www.patreon.com/shirosaidev) page to learn more about how to get access to the OVA downloads.

## Docker

You can set up diskover and diskover-web in docker, there are a few choices for easily running diskover in docker using pre-built images/compose files.

[linuxserver.io](https://linuxserver.io) Docker hub image:
https://hub.docker.com/r/linuxserver/diskover/

[diskover-web](https://github.com/shirosaidev/diskover-web) has Dockerfile with instructions for docker-compose.

## User Guide

[Read the wiki](https://github.com/shirosaidev/diskover/wiki) for more documentation on how to use diskover.

## Discussions/Support

For discussions or support for diskover join the [diskover Slack workspace](https://join.slack.com/t/diskoverworkspace/shared_invite/enQtNzQ0NjE1Njk5MjIyLWI4NWQ0MjFhYzQyMTRhMzk4NTQ3YjBlYjJiMDk1YWUzMTZmZjI1MTdhYTA3NzAzNTU0MDc5NDA2ZDI4OWRiMjM), my username is @shirosai.

You can also post a comment/question on [Google Group](https://groups.google.com/forum/?hl=en#!forum/diskover).

## Bugs

For bugs about diskover, please use the [issues page](https://github.com/shirosaidev/diskover/issues).

## License

See the [license file](https://github.com/shirosaidev/diskover/blob/master/LICENSE).
