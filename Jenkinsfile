pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'Java17'
    }

  stages {

        stage('My Test') {
            steps {
                //sh 'mvn test '
                bat 'mvn test'
            }
         }

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

         stage('Cleanup Docker Image') {
                    steps {
                      script{
                          withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub')]) {

                             // Mevcut Docker container'ını durdurun ve silin
                             // Container mevcut değilse hata vermemesi için '|| true' kullanılır
                            //  sh "docker stop mimaraslan/devops-application:latest || true"
                            //  sh "docker rmi mimaraslan/devops-application:latest || true"
                           //   bat "docker stop mimaraslan/devops-application:latest || true"
                           //   bat "docker rmi mimaraslan/devops-application:latest || true"

                              bat 'docker stop mimaraslan/devops-application:latest'
                              bat 'docker rmi mimaraslan/devops-application:latest'

                        }
                      }
                    }
                }



        stage('Deploy to Kubernetes') {
            steps {
              script{
                  kubernetesDeploy (configs: 'deploymentservice.yaml', kubeconfigId: 'kubernetes')
              }
            }
        }

    }
}