pipeline {
    agent any 

    stages {
        stage("Build Docker Image") {
            steps{
                echo "======== executing Build Docker Image ========"
                script {
                    dockerapp = docker.build("especialistascloud/kubenews:${env.BUILD_ID}",'-f ./src/dockerfile ./src')      
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

        stage("Deploy Kubernetes") {
            environment{
                tag_version = "${env.BUILD_ID}"
            }
            steps{
                echo "======== executing Deploy Kubernetes ========"
                    withKubeConfig ([credentialsID: 'kubeconfig']){
                        sh 'sed -i "s/{{TAG}}/$tag_version/g" ./k8s/deployment.yaml'
                        sh 'kubectl apply -f ./k8s/deployment.yaml'
                    }     
            }    
        }  

    }
}
