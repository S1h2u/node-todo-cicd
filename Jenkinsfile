pipeline {
    agent any
    
    stages {
        stage("Code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/S1h2u/node-todo-cicd.git", branch: "master"
            }
        }
        stage("Build") {
            steps {
                echo "Building the code"
                sh "docker build -t node-app-test-new ."
            }
        }
        stage("Push to docker hub") {
            steps {
                echo "Pushing the image to Docker Hub"
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPass', usernameVariable: 'dockerHubUser')]) {
                        sh 'docker tag node-app-test-new $dockerHubUser/node-app-test-new:latest'
                        sh 'docker login -u $dockerHubUser -p $dockerHubPass'
                        sh 'docker push $dockerHubUser/node-app-test-new:latest'
                    }
                }
            }
        }
        stage("Deploy") {
            steps {
                echo "Deploying the container"
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }
}
