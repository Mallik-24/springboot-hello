pipeline {
    agent any 
    tools {
        maven "3.8.4"
    }
    stages {
        stage('Compile and Clean') { 
            steps {
                // Run Maven on a Unix agent.
                sh "mvn clean compile"
            }
        }
        stage('Deploy') { 
            steps {
                sh "mvn package"
            }
        }
        stage('Build Docker image'){
            steps {
                echo "Hello Java Express"
                sh 'ls'
                sh "docker build -t mallik2001/docker_jenkins_springboot:${BUILD_NUMBER} ."
            }
        }
        stage('Docker Login'){
            steps {
                withCredentials([string(credentialsId: 'mallik2001', variable: 'Dockerpwd')]) {
                    sh "docker login -u mallik2001 -p ${Dockerpwd}"
                }
            }                
        }
        stage('Docker Push'){
            steps {
                sh "docker push mallik2001/docker_jenkins_springboot:${BUILD_NUMBER}"
            }
        }
        stage('Docker deploy'){
            steps {
                sh "docker run -itd -p  8081:8080 mallik2001/docker_jenkins_springboot:${BUILD_NUMBER}"
            }
        }
        stage('Archiving') { 
            steps {
                archiveArtifacts '**/target/*.jar'
            }
        }
    }
}

