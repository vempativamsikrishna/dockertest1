# jenkins auto build and bush image

usermod -aG root jenkins
systemctl reload docker.service
service jenkins restart
service docker restart


pipeline {
    environment {
        registry = "vamsikrishnavempati/vamsikrishna"
        registryCredential = 'dockerhub_id'
        dockerImage = ''
    }
    agent any
    stages {
        stage('Cloning our git') {
            steps {
                git 'https://github.com/vempativamsikrishna/dockertest1.git'
            }
        }
        stage('Building our image') {
            steps{
                script {
                    dockerImage = docker.build registry + ":v$BUILD_NUMBER"
                }
            }
        }
        stage('Pushing our image'){
            steps{
                script{
                    docker.withRegistry( '', registryCredential){
                    dockerImage.push()
                    }
                }
            }
        }
        stage('cleaning up'){
            steps{
                sh "docker rmi $registry:v$BUILD_NUMBER"
            }
        }
    }
}
