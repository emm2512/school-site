pipeline {
    agent any
    stages{
        stage(CodeScan){
            steps{
                sh 'trivy fs . -o file.txt'
                sh 'cat file.txt'
            }
        }
        stage(Ecrlogin){
            steps{
                sh 'aws ecr get-login-password --region us-east-1\
                 | docker login --username AWS --password-stdin 060795940509.dkr.ecr.us-east-1.amazonaws.com'
                
            }
        }  
        stage(Build){
            steps{
                sh 'docker build -t rev-pipeline .'
                
            }
        }
        stage(DockerTag){
            steps{
                sh 'docker tag rev-pipeline:latest 060795940509.dkr.ecr.us-east-1.amazonaws.com/rev-pipeline:latest'
                
            }
        }
        stage(DockerPush){
            steps{
                sh 'docker push 060795940509.dkr.ecr.us-east-1.amazonaws.com/rev-pipeline:latest'
                
            }
        }
    }
    
}