# Taller de jenkins

## Instalación

https://jenkins.io/download/

Instalación con docker

```
docker run \
  -d \
  -p 8080:8080 \
  jenkins/jenkins
```

Instalación con Dockerfile

```Dockerfile
FROM maven:3.6.0-jdk-8 as maven

FROM jenkins/jenkins

COPY --from=maven /usr/share/maven /usr/share/maven

ENV MAVEN_HOME /usr/share/maven
ENV PATH=${MAVEN_HOME}/bin:${PATH}

```

Instalación con docker-compose

```YAML
version: '3'
services:
  jenkins:
    container_name: jenkins
    image: jenkins/jenkins
    ports:
      - "8080:8080"
networks:
  net:
```

## Repositorio ejemplo

https://github.com/spring-projects/spring-petclinic/

```
git clone https://github.com/spring-projects/spring-petclinic.git
```

## Jenkinsfiles

Estrutura Base
```
pipeline { 
    agent any 
    stages {
        stage('Hello World') { 
            steps { 
                echo 'Hello World' 
            }
        }
    }
}
```

post actions

```
pipeline { 
    agent any 
    stages {
        stage('post actions') { 
            steps { 
                sh 'ls -la'
                //sh 'mvn install' 
            }
        }
    }
    post {
        always {
            echo 'Siempre me ejecuto'
        }
        success {
            echo 'Me ejecuto cuando el job es exitoso :D'
        }
        unstable {
            echo 'I am unstable :/'
        }
        failure {
            echo 'Me ejecuto cuando el job falla :('
        }
        changed {
            echo 'Me ejecuto cuando cambio de estado'
        }
    }
}
```

Variables de entorno
```
pipeline { 
    agent any
    environment{
        NOMBRE = 'Andrés'
        APELLIDO = 'Garcia'
    }
    stages {
        stage('Variables de entorno') { 
            steps { 
                echo "Hola ${NOMBRE} ${APELLIDO}"
            }
        }
    }
}
```

Credentials

```
pipeline { 
    agent any
    stages {
        stage('Variables de entorno') {
            environment{
                CLAVE = credentials('secret-text')
            }
            steps { 
                echo "Mi clave es ${CLAVE}"
            }
        }
    }
}
``` 

Integración Continua
```
pipeline { 
    agent any
    environment{
        GIT_URL = 'https://github.com/spring-projects/spring-petclinic.git'
    }
    stages {
        stage('Clone') { 
            steps { 
                sh 'git clone ${GIT_URL}' 
            }
        }
        stage('Build') { 
            steps { 
                sh 'mvn -B package -DskypTests' 
            }
        }
        stage('Test'){
            steps {
                sh 'mvn -B test'
            }
        }
    }
    post {
        always {
            cleanWs()
        }
        success {
            archiveArtifacts artifacts: '**/*.war'
        }
        failure {
            echo 'Error'
        }
    }
}
```