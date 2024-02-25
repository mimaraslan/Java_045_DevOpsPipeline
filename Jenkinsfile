pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'Java17'
    }

    stages {
        stage('Build Maven') {
            steps {
                 checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mimaraslan/Java_045_DevOpsPipeline']])
                //sh 'mvn clean install'
                 bat 'mvn clean install'
            }
        }

        stage('Docker Image') {
            steps {
                // sh 'docker build -t mimaraslan/devops-application .'
                 bat 'docker build -t mimaraslan/devops-application .'
            }
        }


        stage('Docker Image to DockerHub') {
            steps {
              script{
                  withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub')]) {

                 //  sh 'echo docker login -u mimaraslan -p ${dockerhub}'
                 //  sh 'docker push mimaraslan/devops-application:latest'

                   bat 'echo docker login -u mimaraslan -p ${dockerhub}'
                   bat 'docker push mimaraslan/devops-application:latest'
                }
              }
            }
        }


    }
}