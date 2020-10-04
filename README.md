# StackStorm starter packages

This pack provides resources example  [StackStrom](https://stackstorm.com/) packages. For in-depth details, please visit
the excellent [StackStorm documentation](https://docs.stackstorm.com/) page.

| Type          |     Action File        |
|---------------|------------------------|
| Python script | actions/nasa_apod.py   | 

## Prep Instructions

Please visit the following and complete the [prep instructions](https://gist.github.com/nmaludy/21c403d98eaf13f2accfd85e68dadb9c)

## Steps

### Install Stackstorm using Docker
https://github.com/StackStorm/st2-docker
https://docs.stackstorm.com/install/docker.html
git clone https://github.com/StackStorm/st2-docker.git
cd st2-docker

###  Start image and mount directory 
cd /Users/carlos.ferreira1ibm.com/ws/st2-docker
docker-compose exec st2client bash -v /src/webapp:/webapp training/webapp 
docker-compose exec st2client bash
open browser 
docker-compose exec st2client st2 --version

### To SSH Into a Container
docker ps
CONTAINER ID        IMAGE                                  			 COMMAND                  CREATED             STATUS          PORTS        NAMES
a31cd9580b2f        stackstorm/st2actionrunner:3.3dev       "/st2client-startup.…"   3 days ago          Up 3 days                           st2-docker_st2client_1
docker exec -it st2-docker_st2client_1 /bin/bash

###  Create a package 
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
In the st2 image directory /opt/stackstorm/packs/tutorial/actions/
- Test python script and put it in the  actions directory
cd /opt/stackstorm/pack
docker-compose exec st2client st2 run monitor.nasa_apod date=2018-07-04

### Register the action 
st2ctl reload --register-actions

### Run with debug pack monitor with input arguement date
st2 --debug run monitor date=2018-07-04
