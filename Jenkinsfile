pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
            args '-p 3000:3000'
        }
    }
    
    stages {
        
        def myImg
        stage ("Build image") {
            // download the dockerfile to build from
            git 'git@diyvb:repos/dockerResources.git'

            // build our docker image
            myImg = docker.build 'my-image:snapshot'
        }
        
        stage ("Run Build") {
            myImg.inside() {
                sh "docker ps -a"
                }
        }
        
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deploy') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
                sh 'docker ps -a'
                input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)'
                sh './jenkins/scripts/kill.sh' 
            }
        }
    }
}
