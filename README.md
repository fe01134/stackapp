# StackStorm starter packages

This pack provides resources example  [StackStrom](https://stackstorm.com/) packages. For in-depth details, please visit
the excellent [StackStorm documentation](https://docs.stackstorm.com/) page.

| Type          |     Action File        |
|---------------|------------------------|
| Python script | actions/nasa_apod.py   | 

## Prep Instructions

-  Install Docker
-  Install Stackstorm Docker Container

### Steps

#### Install Stackstorm using Docker
https://github.com/StackStorm/st2-docker
https://docs.stackstorm.com/install/docker.html

    git clone https://github.com/StackStorm/st2-docker.git
    cd st2-docker

####  Start Stackstorm client Image and Optionally Mount Host Directory 

    cd /Users/carlos.ferreira1ibm.com/ws/st2-docker
    docker-compose exec st2client bash -v /src/webapp:/webapp training/webapp 
    docker-compose exec st2client bash
    
 Output

    Welcome to StackStorm HA v3.3dev (Ubuntu 18.04.5 LTS GNU/Linux x86_64)
    * Documentation: https://docs.stackstorm.com/
    * Community: https://stackstorm.com/community-signup
    * Forum: https://forum.stackstorm.com/
     * Enterprise: https://stackstorm.com/#product
    
     Here you can use StackStorm CLI. Examples:
       st2 action list --pack=core
       st2 run core.local cmd=date
       st2 run core.local_sudo cmd='apt-get update' --tail
       st2 execution list
       
- Open browser with URL localhost to
- Login using  user id `st2admin`  and password `Ch@ngeMe`

docker-compose exec st2client st2 --version

#### SSH Into a Stackstorm st2client_1 Container

Open a terminal window on your host.  Check if the Stackstorm docker container is installed and running.

    docker ps

Output

    CONTAINER ID        IMAGE                                  			 COMMAND                  CREATED             STATUS          PORTS        NAMES
    a31cd9580b2f        stackstorm/st2actionrunner:3.3dev       "/st2client-startup.…"   3 days ago          Up 3 days                           st2-docker_st2client_1

Invoke command line shell to run Stackstorm commands

    docker exec -it st2-docker_st2client_1 /bin/bash

## Create Your First Stackstorm Package 
Using these directions 
https://docs.stackstorm.com/reference/packs.html

###  Install tutorial actions and rules  (from within the stackstorm container)
https://github.com/EncoreTechnologies/stackstorm-tutorial/blob/master/doc/01_actions_script.md

cd /Users/carlos.ferreira1ibm.com/ws/st2-docker   (Docker diretory for stackstorm)
docker-compose exec st2client st2 pack install https://github.com/fe01134/stackapp.git
        
st2 pack install https://github.com/fe01134/stackapp.git
st2 pack install https://github.com/encoretechnologies/stackstorm-tutorial.git

	[  running  ] init_task
	[ succeeded ] init_task
	[ succeeded ] download_pack
	[ succeeded ] make_a_prerun
	[ succeeded ] get_pack_dependencies
	[ succeeded ] check_dependency_and_conflict_list
	[ succeeded ] install_pack_requirements
	[ succeeded ] get_pack_warnings
	[ succeeded ] register_pack

+-------------+-------------------------------------------------------+
| Property    | Value                                                 |
+-------------+-------------------------------------------------------+
| ref         | tutorial                                              |
| name        | tutorial                                              |
| description | Hands-on tutorial for getting started with StackStorm |
| version     | 0.1.0                                                 |
| author      | Encore Technologies                                   |
+-------------+-------------------------------------------------------+

###  Install tutorial actions and rules  (from outside the stackstorm container)

    cd /Users/carlos.ferreira1ibm.com/ws/st2-docker
    docker-compose exec st2client st2 pack install https://github.com/fe01134/stackapp.git

### Test run the  action 
In the st2 image directory /opt/stackstorm/packs/monitor/actions/
Test python script and put it in the  actions directory

    cd /opt/stackstorm/pack
    docker-compose exec st2client st2 run monitor.nasa_apod date=2018-07-04

### Register the action 

    st2ctl reload --register-actions

### Run with debug pack monitor with input arguement date

    st2 --debug run monitor date=2018-07-04

## Install and Use 
First, install the rabbitmq pack from the public StackStorm exchange.

    st2 pack install rabbitmq

Configure RabbitMQ queue
Create a queue in RabbitMQ where messages will be sent

    rabbitmqadmin declare exchange name=demo type=topic durable=false
    rabbitmqadmin declare queue name=demoqueue
    rabbitmqadmin declare binding source=demo destination=demoqueue routing_key=demokey

Test out the RabbitMQ pack
st2 run rabbitmq.publish_message host=127.0.0.1 exchange=demo exchange_type=topic routing_key=demokey message="test"
Read from the queue to see if our message was delivered:

rabbitmqadmin get queue=demoqueue count=99