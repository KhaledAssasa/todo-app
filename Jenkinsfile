pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/KhaledAssasa/todo-app.git'
                sh "docker build -t todo-app ."
                sh "docker run todo-app ."
            }
            }
        }
    }
