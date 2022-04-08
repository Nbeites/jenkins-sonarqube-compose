# set up jenkins+sonarqube+postgres on host machine

    The host machine must have at least 4gb RAM, and for integration with
    sonarqube the recommended is 8gb RAM.

    Change max virtual memory for host machine : 
        - sudo sysctl -w vm.max_map_count=262144

    install docker on host machine : 
        - sudo apt install docker
        - sudo apt install docker-compose



    ##Install jenkins in host machine

        To Install jenkins-docker in remote machine inside a docker container:

        (OR wget https://raw.githubusercontent.com/Nbeites/jenkins-pipeline-sample/main/jenkins-docker-master/Dockerfile)
        
        - docker build -t jenkins-docker .
        - docker run -p 8080:8080 -p 50000:50000 -v /var/jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock --name jenkins -d jenkins-docker
        - docker exec -it jenkins bash (-it is to open a terminal)

        (To see directory system : docker exec -t -i mycontainer /bin/bash)

        Note : to be functional, docker ps command must run correctly, if it does, means that the docker ps
        is communicating with socket present in /var/run/docker.sock in the host machine

        Check socket: ls -ahl /var/run/docker.sock (this command can run inside container or in host machine (both have to have the socket))

            To see the group id of docker (host machine and/or container) :
                - grep docker /etc/group

            NOTE : THE 2 GROUPS MUST HAVE THE SAME GID (998 as in docker:x:998:)
            IMPORTANT NOTE: THERE ARE 2 GROUPS, THE HOST MACHINE DOCKER GROUP AND THE CONTAINER DOCKER GROUP
            EXAMPLE : host machine -> docker:x:998:root ; container -> docker:x:998:jenkins (THIS WORKS), just run the DockerFile that is 
            in this project in jenkins-docker

            - ADDAPT THE DOCKERFILE DEPOENDING ON GROUP ID DOCKER OF HOST MACHINE

            If does not work, try executing the following command outside container : sudo systemctl restart docker
            And try again...

            If not resolves, then see what comes out of this command :
                - grep docker /etc/group (inside container)
                    If it's like docker:x:998:
                        - sudo usermod -a -G docker jenkins (to add jenkins to docker group) (supposedly is already inside dockerfile)


    ##First Jenkins Config

        first jenkins config:
            search initial password in: /var/jenkins_home/secrets/initialAdminPassword

        Finish installation



    ##Run docker-compose file: (with sonarqube and postgresql for sonarqube)

        Install docker compose:
        - sudo apt-get docker-compose

        - wget https://raw.githubusercontent.com/Nbeites/jenkins-sonarqube-compose/main/docker-compose.yml

        - docker-compose up -d