pipeline {
    agent any
    stages {
        stage('Tests') {
            steps {
//                 script {
//                    docker.image('node:10-stretch').inside { c ->
                        echo 'Building..'
                        sh 'npm install'
                        echo 'Testing..'
                        sh 'npm test'
//                         sh "docker logs ${c.id}"
//                    }
//                 }
            }
        }
        stage('Build and push docker image') {
            steps {
                script {
                    def dockerImage = docker.build("135331/todo:latest")
                    docker.withRegistry('', 'demo-docker') {
                        dockerImage.push('master')
                    }
                }
            }
        }
        stage('Deploy to remote docker host') {
            environment {
                DOCKER_HOST_CREDENTIALS = credentials('demo-docker')
            }
            steps {
                script {
//                     sh 'docker login -u $DOCKER_HOST_CREDENTIALS_USR -p $DOCKER_HOST_CREDENTIALS_PSW 127.0.0.1:2375'
                    sh 'docker pull 135331/todo:latest'
                    sh 'docker stop node-demo'
                    sh 'docker rm node-demo'
                    sh 'docker rmi 135331/todo:current'
                    sh 'docker tag 135331/todo:master 135331/todo:current'
                    sh 'docker run -d --name node-demo -p 80:3000 135331/todo:current'
                }
            }
        }
    }
}
