# ubuntu-java-mysql

Docker build file for ubuntu 16.04, oracle-java8, mysql-server, maven.

### used in jenkins pipeline 
```groovy
        docker.image("nimrod007/ubuntu-jdk-mysql-mvn").inside('-u root') {

            stage("Create MYSQL-DB") {
                try {
                    sh 'service mysql start'
                    sh 'mysql -uroot -e "create database logger"'
                    sh 'mysql -uroot -e "CREATE USER user@localhost"'
                    sh 'mysql -uroot -e "GRANT ALL PRIVILEGES ON *.* TO \"user\"@\"localhost\" IDENTIFIED BY \'pass\' WITH GRANT OPTION"'
                } catch (e) { echo "error is mysql creation"
                } finally {
                    sh 'cat /var/log/mysql/*.log'
                }
            }

            stage("Build & Test") {
                sh 'mvn clean install'
            }
        }

```

Suitable for running maven builds, tests and deploying to aws.

Docker images at - https://hub.docker.com/r/nimrod007/ubuntu-jdk-mysql-mvn/

**References**

- Dockerfile Best Practices - https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/#/apt-get
- dockerfile/ubuntu - https://github.com/dockerfile/ubuntu/blob/master/Dockerfile
- dockerfile/oracle-java8 - https://github.com/dockerfile/java/blob/master/oracle-java8/Dockerfile
- dockerfile/mysql - https://github.com/dockerfile/mysql/blob/master/Dockerfile

** build **
```bash
docker build --tag nimrod007/ubuntu-jdk-mysql-mvn:latest .
docker push nimrod007/ubuntu-jdk-mysql-mvn:latest
```
