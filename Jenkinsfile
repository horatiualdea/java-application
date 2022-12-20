def remote = [:]
remote.name = "WebServer"
remote.host = "3.122.231.61"
remote.allowAnyHosts = true

withCredentials([sshUserPrivateKey(credentialsId: 'webserverpk', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
    remote.user = userName
    remote.identityFile = identity
}

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
            sshCommand remote: remote, command: 'docker run --name helloworld public.ecr.aws/l9o2c9u6/helloworld:1.0'   
        }
    }
}
