<h1 align="center" style="border-bottom: none">
   Install Sentry with docker on Ubuntu 22.4
</h1>
<hr><br>

Sentry is a free and open-source error-tracking platform that monitors and fixes crashes in real-time. It allows software developers to see what matters, solve quicker, and learn continuously about their applications. This platform provides real-time insight into production deployments with info to reproduce and fix crashes. Sentry supports all major languages and frameworks and integrates with your favorite apps and services.

This tutorial will show you how to install Sentry Error Tracking System with Docker on Ubuntu 22.04.

## Prerequisites
* A server running Ubuntu 22.04

* A valid domain name is pointed to your server IP

* A root password is configured on your server

## Install Sentry
First, download the latest version of Sentry from the Git repository using the following command:

```bash
git clone https://github.com/getsentry/onpremise
```

Once the download is complete, change the directory to the downloaded directory and run the Sentry installation script to start the installation.

```bash
cd onpremise

bash install.sh
```

During the installation, you will be asked to create an admin account as shown below:

```
Here's the info we may collect:

  - OS username
  - IP address
  - install log
  - runtime errors
  - performance data

Thirty (30) day retention. No marketing. Privacy policy at sentry.io/privacy.

Would you like to create a user account now? [Y/n]: y

Email: Email: hitjethva81@gmail.com
Password: 
Repeat for confirmation: 
Added to organization: sentry
User created: hitjethva81@gmail.com
Creating missing DSNs
Correcting Group.num_comments counter

-----------------------------------------------------------------

You're all done! Run the following command to get Sentry running:

  docker-compose up -d

-----------------------------------------------------------------
```

Next, verify all the downloaded images using the following command.

```bash
docker images
```

You should see the following output:

```
REPOSITORY                               TAG             IMAGE ID       CREATED             SIZE
sentry-cleanup-self-hosted-local         latest          04fa0fce18f0   4 minutes ago       908MB
symbolicator-cleanup-self-hosted-local   latest          6837f5f48e6c   5 minutes ago       180MB
snuba-cleanup-self-hosted-local          latest          242b7b248e1c   5 minutes ago       486MB
sentry-self-hosted-local                 latest          101b00356aa6   5 minutes ago       907MB
sentry-self-hosted-jq-local              latest          83c66fd3f78f   6 minutes ago       82.5MB
getsentry/sentry                         nightly         cf0f404d102e   About an hour ago   907MB
getsentry/snuba                          nightly         fc6c2d286bf8   8 hours ago         484MB
getsentry/relay                          nightly         43cd2ba5497c   5 days ago          242MB
busybox                                  latest          66ba00ad3de8   6 days ago          4.87MB
tianon/exim4                             latest          12842ac621c1   2 weeks ago         158MB
debian                                   bullseye-slim   dd94cb611937   2 weeks ago         80.5MB
getsentry/sentry-cli                     latest          a585383ff864   2 weeks ago         26.3MB
getsentry/symbolicator                   nightly         80d9b41cd195   3 weeks ago         178MB
nginx                                    1.22.0-alpine   5685937b6bc1   3 months ago        23.5MB
postgres                                 9.6             027ccf656dc1   11 months ago       200MB
confluentinc/cp-kafka                    5.5.0           efc480c1c89c   15 months ago       598MB
confluentinc/cp-zookeeper                5.5.0           ddeb961d8e80   15 months ago       598MB
redis                                    6.2.4-alpine    500703a12fa4   18 months ago       32.3MB
memcached                                1.6.9-alpine    a0132b3398e4   18 months ago       8.09MB
curlimages/curl                          7.77.0          e062233fb4a9   19 months ago       8.26MB
maxmindinc/geoipupdate                   v4.7.1          8ec32cc727c7   21 months ago       10.6MB
clickhouse-self-hosted-local             latest          abe55fc6544d   2 years ago         497MB
yandex/clickhouse-server                 20.3.9.70       abe55fc6544d   2 years ago         497MB
```

### Launch Sentry Container
At this point, Sentry is installed. You can now start the Sentry container using the following command:

```bash
docker-compose up -d
```

This will start all the containers for Sentry as shown below:

```
Starting sentry_onpremise_memcached_1            ... done
Starting sentry_onpremise_redis_1                ... done
Starting sentry_onpremise_symbolicator_1         ... done
Creating sentry_onpremise_symbolicator-cleanup_1 ... done
Starting sentry_onpremise_zookeeper_1            ... done
Starting sentry_onpremise_clickhouse_1           ... done
Starting sentry_onpremise_smtp_1                 ... done
Starting sentry_onpremise_postgres_1             ... done
Starting sentry_onpremise_kafka_1                ... done
Starting sentry_onpremise_snuba-consumer_1          ... done
Starting sentry_onpremise_snuba-outcomes-consumer_1 ... done
Starting sentry_onpremise_snuba-api_1               ... done
Starting sentry_onpremise_snuba-sessions-consumer_1 ... done
Starting sentry_onpremise_snuba-replacer_1          ... done
Creating sentry_onpremise_snuba-cleanup_1           ... done
Creating sentry_onpremise_relay_1                   ... done
Creating sentry_onpremise_web_1                     ... done
Creating sentry_onpremise_post-process-forwarder_1  ... done
Creating sentry_onpremise_cron_1                    ... done
Creating sentry_onpremise_sentry-cleanup_1          ... done
Creating sentry_onpremise_worker_1                  ... done
Creating sentry_onpremise_ingest-consumer_1         ... done
Creating sentry_onpremise_nginx_1                   ... done
```

You can verify the status of all containers using the following command:

```bash
docker-compose ps
```

You should see the following output:

```
Ports                
----------------------------------------------------------------------------------------------------------------------------------------------
sentry-self-hosted_clickhouse_1                           /entrypoint.sh                   Up (healthy)   8123/tcp, 9000/tcp, 9009/tcp        
sentry-self-hosted_cron_1                                 /etc/sentry/entrypoint.sh  ...   Up             9000/tcp                            
sentry-self-hosted_geoipupdate_1                          /usr/bin/geoipupdate -d /s ...   Exit 1                                             
sentry-self-hosted_ingest-consumer_1                      /etc/sentry/entrypoint.sh  ...   Up             9000/tcp                            
sentry-self-hosted_kafka_1                                /etc/confluent/docker/run        Up (healthy)   9092/tcp                            
sentry-self-hosted_memcached_1                            docker-entrypoint.sh memcached   Up (healthy)   11211/tcp                           
sentry-self-hosted_nginx_1                                /docker-entrypoint.sh ngin ...   Up             0.0.0.0:9000->80/tcp,:::9000->80/tcp
sentry-self-hosted_post-process-forwarder-errors_1        /etc/sentry/entrypoint.sh  ...   Up             9000/tcp                            
sentry-self-hosted_post-process-forwarder-                /etc/sentry/entrypoint.sh  ...   Up             9000/tcp                            
transactions_1                                                                                                                                
sentry-self-hosted_postgres_1                             /opt/sentry/postgres-entry ...   Up (healthy)   5432/tcp                            
sentry-self-hosted_redis_1                                docker-entrypoint.sh redis ...   Up (healthy)   6379/tcp                            
sentry-self-hosted_relay_1                                /bin/bash /docker-entrypoi ...   Up             3000/tcp                            
sentry-self-hosted_sentry-cleanup_1                       /entrypoint.sh 0 0 * * * g ...   Up             9000/tcp                            
sentry-self-hosted_smtp_1                                 docker-entrypoint.sh exim  ...   Up             25/tcp                              
sentry-self-hosted_snuba-api_1                            ./docker_entrypoint.sh api       Up             1218/tcp                            
sentry-self-hosted_snuba-cleanup_1                        /entrypoint.sh */5 * * * * ...   Up             1218/tcp                            
sentry-self-hosted_snuba-consumer_1                       ./docker_entrypoint.sh con ...   Up             1218/tcp                            
sentry-self-hosted_snuba-outcomes-consumer_1              ./docker_entrypoint.sh con ...   Up             1218/tcp                            
sentry-self-hosted_snuba-replacer_1                       ./docker_entrypoint.sh rep ...   Up             1218/tcp                            
sentry-self-hosted_snuba-sessions-consumer_1              ./docker_entrypoint.sh con ...   Up             1218/tcp                            
sentry-self-hosted_snuba-subscription-consumer-events_1   ./docker_entrypoint.sh sub ...   Up             1218/tcp                            
sentry-self-hosted_snuba-subscription-consumer-           ./docker_entrypoint.sh sub ...   Up             1218/tcp                            
transactions_1                                                                                                                                
sentry-self-hosted_snuba-transactions-cleanup_1           /entrypoint.sh */5 * * * * ...   Up             1218/tcp                            
sentry-self-hosted_snuba-transactions-consumer_1          ./docker_entrypoint.sh con ...   Up             1218/tcp                            
sentry-self-hosted_subscription-consumer-events_1         /etc/sentry/entrypoint.sh  ...   Up             9000/tcp                            
sentry-self-hosted_subscription-consumer-transactions_1   /etc/sentry/entrypoint.sh  ...   Up             9000/tcp                            
sentry-self-hosted_symbolicator-cleanup_1                 /entrypoint.sh 55 23 * * * ...   Up             3021/tcp                            
sentry-self-hosted_symbolicator_1                         /bin/bash /docker-entrypoi ...   Up             3021/tcp                            
sentry-self-hosted_web_1                                  /etc/sentry/entrypoint.sh  ...   Up (healthy)   9000/tcp                            
sentry-self-hosted_worker_1                               /etc/sentry/entrypoint.sh  ...   Up             9000/tcp                            
sentry-self-hosted_zookeeper_1                            /etc/confluent/docker/run        Up (healthy)   2181/tcp, 2888/tcp, 3888/tcp        
```

Once you are finished, you can proceed to the next step.

## Access Sentry Web UI
At this point, Sentry is started and listening on port 9000. Now, open your web browser and type the URL http://your-server-ip:9000 to access the Sentry dashboard. You will be redirected to the Sentry login page.

Provide your admin username, password and click on the Login button. 

Provide your Sentry URL, email address, and SMTP details, and click on the Continue button.


