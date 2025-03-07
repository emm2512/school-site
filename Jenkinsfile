pipeline {
    agent any
    stages{
        stage('CodeScan'){
            steps{
                sh 'trivy fs . -o file.txt'
                sh 'cat file.txt'
            }
        }
        stage('Ecrlogin'){
            steps{
                sh 'aws ecr get-login-password --region us-east-1 | \
                 docker login --username AWS --password-stdin 060795940509.dkr.ecr.us-east-1.amazonaws.com'
                
            }
        }  
        stage('Build'){
            steps{
                sh 'docker build -t rev-pipeline .'
                sh 'docker build -t newversion .'
                sh 'docker images'
                
            }
        }
        stage('DockerTag'){
            steps{
                sh 'docker tag rev-pipeline:latest 060795940509.dkr.ecr.us-east-1.amazonaws.com/rev-pipeline:latest'
                sh 'docker tag rev-pipeline:latest 060795940509.dkr.ecr.us-east-1.amazonaws.com/rev-pipeline:v1.$BUILD_NUMBER'
            }
        }
        stage('DockerPush'){
            steps{
                sh 'docker push 060795940509.dkr.ecr.us-east-1.amazonaws.com/rev-pipeline:latest'
                sh 'docker push 060795940509.dkr.ecr.us-east-1.amazonaws.com/rev-pipeline:v1.$BUILD_NUMBER'
                
            }
        }
        stage('CodeTest'){
           steps{ 
            sh 'docker run -itd --name web3 -p 80:80 newversion' 
            sh 'docker ps'
           }
        }
    }
    
}