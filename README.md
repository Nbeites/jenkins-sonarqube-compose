# set up jenkins+sonarqube+postgres on host machine

    install docker on host machine : 
        sudo apt install docker
        sudo apt install docker-compose



###Install jenkins in host machine

    - wget https://raw.githubusercontent.com/Nbeites/jenkins-pipeline-sample/main/jenkins-install-digitalOcean/install_jenkins.sh

    - bash install_jenkins.sh

    After Install:
        - cat /var/jenkins_home/secrets/initialAdminPassword (to check initial password for jenkins)
        - cat /var/jenkins_home/workspace (to see projects)


###First Jenkins Config

    first jenkins config:
        search initial password in: /var/jenkins_home/secrets/initialAdminPassword

    Finish installation



###Run docker-compose file: (with sonarqube and postgresql for sonarqube)

    Install docker compose:
       - sudo apt-get docker-compose



    - docker-compose up -d