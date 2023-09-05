# jenkins auto build and push image

//usermod -aG root jenkins
// systemctl reload docker.service
// service jenkins restart
// service docker restart

// #for connecting to docker master from jenkins serveruse below command

// nano /lib/systemd/system/docker.service
// ExecStart=/usr/bin/dockerd -H unix:// -H tcp://0.0.0.0:2375
// systemctl daemon-reload
// systemctl restart docker
// docker -H tcp://10.1.1.1:2375(master ip address) docker commands


// pipeline {
//     environment {
//         registry = "vamsikrishnavempati/vamsikrishna"
//         registryCredential = 'dockerhub_id'
//         dockerImage = ''
//     }
//     agent any
//     stages {
//         stage('Cloning our git') {
//             steps {
//                 git 'https://github.com/vempativamsikrishna/dockertest1.git'
//             }
//         }
//         stage('Building our image') {
//             steps{
//                 script {
//                     dockerImage = docker.build registry + ":v$BUILD_NUMBER"
//                 }
//             }
//         }
//         stage('Pushing our image'){
//             steps{
//                 script{
//                     docker.withRegistry( '', registryCredential){
//                     dockerImage.push()
//                     }
//                 }
//             }
//         }
//         stage('cleaning up'){
//             steps{
//                 sh "docker rmi $registry:v$BUILD_NUMBER"
//             }
//         }
//         stage('deploying container'){
//             steps{
//                 sh "docker -H tcp://10.1.1.30:2375 service rm nginx"
//                 sh "docker -H tcp://10.1.1.30:2375 service create --name nginx -p 9000:80 $registry:v$BUILD_NUMBER"
//             }
//         }
//     }
// }
