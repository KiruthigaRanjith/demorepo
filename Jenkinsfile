pipeline {
    agent any
    tools{
        maven 'maven_4'
    }
    environment {
        DOCKER_CREDENTIALS = credentials('docker-credentials')  // Replace with your credential ID
    }
    stages {
        stage('Build maven') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/KiruthigaRanjith/demorepo']])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image') {
            steps {
                script {
                    sh 'docker build -t kiruthiga1411/cicd:demo-0.0.1-snapshot .'
                }
            }
        }
        stage('Push docker image to hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-credentials',
                                                      usernameVariable: 'DOCKER_USERNAME',
                                                      passwordVariable: 'DOCKER_PASSWORD')]){
                          sh 'docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}'
                    }
                    sh 'docker push kiruthiga1411/cicd:demo-0.0.1-snapshot'
                }
            }
        }

    }
}
