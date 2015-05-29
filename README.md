## Apache Log Analysis in 5 Mintues with ELK & Docker

(ELK: Elasticsearch, Logstash & Kibana)

A simple demo to show how docker & docker-compose make it easy to run useful services.

In this demo we are going to spin up the following applications:

* logstash
* elasticsearch
* kibana
* drupal

with only a 15-line yaml file and one command!

#### Dependencies
* docker (i'm using v1.4.1)
* netcat, or one of it's ilk (nc, ncat, socat)

Not going to go into details here on docker installation, but there are many options:

* on linux you can most likely use your favorite package manager
* on mac (I think) you'll need something called [boot2docker](http://boot2docker.io/)
* install into a vagrant vm
* even Windows! (but don't ask me how!)

### 1. Clone this repo

1. `git clone https://github.com/lbjay/apache-elk-in-five-minutes.git`
1. `cd apache-elk-in-five-minutes`

### 2. Make & activate a virtualenv (optional but recommended)

1. `virtualenv venv`
1. `source venv/bin/activate`

### 3. Install docker-compose

Either `pip install -r requirements.txt` or `pip install docker-compose`

### 4. Create a .env file

Create a file called `.env` with the following contents:

```
LOGSTASH_CONFIG_URL=https://gist.githubusercontent.com/lbjay/98a62625f9a5570f8c15/raw/226ee25afbd60b27b9b00ae74c0cd2d03c2f1b01/logstash.conf
```

### 5. Run the container

[docker-compose](https://docs.docker.com/compose/)  makes this easy. Just run...

    %> docker-compose up -d

Alternately, [docker-compose](https://docs.docker.com/compose/) is just a tool for orchestrating multiple docker containers, so you can also just execute directly like so...

    %> docker run -d --env_file=.env -p "9200:9200" -p "9292:9292" -p "3333:3333" pblittle/docker-logstash
    %> docker run -d -p "80:8080" drupal:latest


### 6. Check that the services are running

`docker ps` will give you a list of running containers. You should see 2.

Browse to...

* elasticsearch: [http://localhost:9200]()
* kibana: [http://localhost:9292]()
* drupal: [http://localhost:8080]()

### 7a. Hey, lets pipe our drupal apache logs into ELK!

1. get the drupal container id from `docker ps`.
1. run `docker logs -f [container_id] 2>&1 | nc localhost 3333`

### 7b. Or pipe an apache log file from somewhere else into logstash

`cat access_log | nc localhost 3333`

### 8. Kibana

You should now be able to go back and forth between [drupal](http://localhost:8080) and [kibana](http://localhost:9292) and see the drupal apache log events populating the default dashboard.

