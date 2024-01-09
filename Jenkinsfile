pipeline {
    agent any
    
    tools {
        maven 'my_maven'
    }
    
    environment {
        GITNAME = 'programusyeon'
        GITEMAIL = 'programusyeon@gmail.com'
        GITWEBADD = 'https://github.com/programsuyeon/sb_code.git'
        GITSSHADD = 'git@github.com:programsuyeon/sb_code.git'
        GITCREDENTIAL = 'git_cre'
        
        DOCKERHUB = 'programusyeon/eks_spring'
        DOCKERHUBCREDENTIAL = 'docker_cre'
    }
    
    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
        
        stage('code build') {
            steps {
                sh "mvn clean package"
            }
        }
        stage('image build') {
            steps {
                sh "docker build -t ${DOCKERHUB}:${currentBuild.number} ."
                // currentBuild.number = 젠킨스가 제공하는 빌드넘버 변수
                // oolralra/spring:1 같은 형태로 빌드가 될것
            }
        }
        stage('image push') {
            steps {
                withDockerRegistry(credentialsId:DOCKERHUBCREDENTIAL, url: ''){
                    sh "docker push ${DOCKERHUB}:${currentBuild.number}"
                    "sh "docker push ${DOCKERHUB}:latest"
                }
            }
            
            post{
                failure {
                    echo 'docker image push failure'
                    sh "docker image rm -f ${DOCKERHUB}:${currentBuild.number}"
                    sh "docker image rm -f ${DOCKERHUB}:latest"
                }
                
                success {
                    echo 'docker image push success'
                    sh "docker image rm -f ${DOCKERHUB}:${currentBuild.number}"
                    sh "docker image rm -f ${DOCKERHUB}:latest"
                }
            }
        }
    }
}
