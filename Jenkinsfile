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
                bat 'docker tag helloworld:latest public.ecr.aws/l9o2c9u6/helloworld:latest'
                bat 'docker push public.ecr.aws/l9o2c9u6/helloworld:latest'
            }    
        }

        stage('Run') {
            steps {
                bat 'docker images'
                bat 'docker run -t helloworld:1.0'
            }
        }
    }
}
