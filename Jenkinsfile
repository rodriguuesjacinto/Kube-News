pipeline {
    agent any 

    stages {
        stage("Build Docker Image") {
            steps{
                echo "======== executing Build Docker Image ========"
                script {
                    dockerapp = docker.build("especialistascloud/kubenews:${env.BUILD_ID}",'-f ./src/dokerfile ./src')      
                }
            }    
        }

        stage("Puch Docker Image") {
            steps{
                echo "========executing Puch Docker Image========"
                script{
                    docker.withRegistry('https://registry.hub.docker.com','dockerhub'){
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }
    }
}
