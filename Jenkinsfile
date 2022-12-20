pipeline {
    
    agent any
    environment {
        ECR_TOKEN = credentials('ecr-token')
    }
    stages {
        stage('Build') {
            steps {
                withMaven(maven : 'apache-maven-3.8.6') {
                                bat 'mvn clean install'
                 }
            }
        }
        
        stage('Publish') {
            steps {
                bat 'docker login --username AWS --password %ECR_TOKEN% public.ecr.aws/l9o2c9u6'
                bat 'docker tag helloworld:1.0 public.ecr.aws/l9o2c9u6/helloworld:1.0'
                bat 'docker push public.ecr.aws/l9o2c9u6/helloworld:1.0'
            }    
        }

        stage('Run') {
            steps {
                bat 'docker images'
                bat 'docker run -t helloworld:1.0'
            }
        }
        stage('Deploy') {
            steps {    
                withCredentials([sshUserPrivateKey(credentialsId: "webserverpk", keyFileVariable: 'keyfile')]) {
                    bat 'ssh -o StrictHostKeyChecking=no -i %keyfile% ubuntu@ec2-3-122-231-61.eu-central-1.compute.amazonaws.com sudo docker images'
            
                }
            }
        }
    }
}


