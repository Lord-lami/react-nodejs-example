#!/usr/bin/env groovy

pipeline {
    agent any
    environment {
        IMAGE_NAME = 'mideol/java-maven-app:nodejs1.0'
    }
    stages {
        stage('build app') {
            steps {
                script {
                    echo "building the application..."
                }
            }
        }
        stage('build image') {
            steps {
                script {
                    echo "building the docker image...."
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh "docker build -t ${IMAGE_NAME} ."
                        sh 'echo $PASS | docker login -u $USER --password-stdin'
                        sh "docker push ${IMAGE_NAME}"
                    }
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    echo 'deploying docker image to EC2...'
                    // if Jenkins server is not on EC2 server run with sshAgent firs
                    sh "docker run -d -p 80:80 ${IMAGE_NAME}"
                }
            }
        }
    }
}
