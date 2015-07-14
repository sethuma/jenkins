# jenkins
Each docker folder has README file that includes the following instructions.

	Version of OS and Docker that was built / tested.
	

File Lists:

 boot2docker.sh - used to set env parameters useful for boot2docker daemon
 Dockerfile - used to build jenkins container
 Dockerfile.ubuntu - Used to build docker container on top of ubuntu. Original Dockerfile should be replaced with Dockerfile.ubuntu to create ubuntu OS container.
 ./base - Base directory to that contains files to build base centos container. 
 jenkins.sh - Can be used to create container from a jenkins job
 

docker run -it \
--name jenkins-data \
-h jenkins-data \
-v /var/lib/jenkins \
-v /var/lib/jenkins_slave_key \
-e ROLE="slave" \
appcontainer_name/jenkins


data:
  image: appcontainers/jenkins
  hostname: jenkins-data
  stdin_open: true
  tty: true
  restart: always
  volumes:
  - /var/lib/jenkins
  - /var/lib/jenkins_slave_key
  environment:
  - ROLE=slave
  - SSH_PASS=jenkinspassword
  command: /bin/bash

web:
  image: appcontainers/jenkins
  hostname: jenkins
  stdin_open: true
  tty: true
  restart: always
  ports:
  - "8080:8080"
  environment:
  - ROLE=master
  - SSH_PASS=jenkinspassword
  volumes_from:
  - data
  command: /bin/bash
