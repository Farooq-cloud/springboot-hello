pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('Docker-hub') // Define your Docker Hub credentials in Jenkins
        def currentDate = new Date().format("yyyy-MM-dd-HH-mm-ss")
    }
    stages {
        stage('Compile and Clean') { 
            steps {
                // Run Maven on a jenkins server.
              
                sh "mvn clean compile"
            }
        }
        stage('deploy') { 
            
            steps {
                sh "mvn package"
            }
        }
        stage('Build Docker image'){
          
            steps {
                echo "Hello Farooq"
                sh 'ls'
                sh 'sudo docker build -t farooq786/springboot:${currentDate}_${BUILD_NUMBER} .'
            }
        }
        stage('Docker Login'){
            
            steps {
                 withCredentials([usernamePassword(credentialsId: 'Docker-hub', usernameVariable: 'DOCKERHUB_CREDENTIALS_USR', passwordVariable: 'DOCKERHUB_CREDENTIALS_PSW')]) {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                }
            }                
        }
        stage('Docker Push and Deploy'){
            steps {
                sh 'sudo docker push farooq786/springboot:${currentDate}_${BUILD_NUMBER}'
                sh 'sudo docker run -itd -p 8081:8080 --name springboot farooq786/springboot:${currentDate}_${BUILD_NUMBER}'
            }
        }

        stage('Archving') { 
            steps {
                 archiveArtifacts '**/target/*.jar'
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
