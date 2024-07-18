pipeline {
    agent any
     environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    stages {
        
        stage("code"){
            steps{
                git url: "https://github.com/streak2508/node-todo-cicd.git", branch: "main"
                echo 'code clone ho gaya'
            }
        }
        stage("build and test"){
            steps{
                sh "docker build -t ${env.dockerHubUser}/my-node-app:${BUILD_NUMBER} ."
                echo 'code build bhi ho gaya'
            }
        }
        
        stage("push"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHubCreds",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/my-node-app:${BUILD_NUMBER}"
                echo 'image push ho gaya'
                }
            }
        }
        stage("deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
                echo 'deployment ho gayi'
            }
        }
    }
}
