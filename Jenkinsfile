pipeline {
    agent any

    environment {
        tag = sh(returnStdout: true, script: "git rev-parse --short=7 HEAD").trim()
    }

    stages {
        stage('Build') {
            steps {
                echo 'Starting to build weather-api docker image..'
                dir ("weather-api"){
                    sh 'docker build -t localhost:8082/repository/docker/weather-api:${tag} .'
                    sh 'docker login -u admin -p admin http://localhost:8082'
                    sh 'docker image push localhost:8082/repository/docker/weather-api:${tag}'
                }
            }
        }
    }
}