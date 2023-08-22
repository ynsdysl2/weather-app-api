pipeline {
    agent any

    environment {
        tag = sh(returnStdout: true, script: "git rev-parse --short=7 HEAD").trim()
    }

    stages {
        stage('Build') {
            steps {
                echo 'Starting to build weather-api docker image...'
                dir ("weather-api"){
                    sh 'docker build -t localhost:8082/repository/docker/weather-api:${tag} .'
                    sh 'docker login -u admin -p admin http://localhost:8082'
                    sh 'docker image push localhost:8082/repository/docker/weather-api:${tag}'
                    sh 'docker system prune -af'
                    
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Starting to deploy weather-api...'
                dir ("k8s"){
                    sh 'sed -i "s/TAG/${tag}/g" deployment.yaml'
                    sh 'sudo kubectl apply -f deployment.yaml'
                    sh 'sudo kubectl apply -f service.yaml'
                    sh 'sudo kubectl apply -f ingress.yaml'
                }
            }
        }
    }
}
