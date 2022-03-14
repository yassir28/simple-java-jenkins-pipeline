pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                /*sh"sudo javac app.java"
                sh "java HelloWorld"*/
                sh "sudo docker build -t my-java-app ."
                sh "sudo docker run -it --rm --name my-running-app my-java-app"

            }
            
        }
        stage('Test') {
            steps {
                echo 'Testing'
                sh 'make check || true' 
                junit '**/target/*.xml' 
            }
        }
        stage('Deploy') {
            when {
              expression {
                currentBuild.result == null || currentBuild.result == 'SUCCESS' 
              }
            }
            steps {
                sh 'make publish'
            }
        }
        
        stage('Build and Push Docker Image...') {
            steps {
                script {
                    // DOCKER HUB
                      
                    /* Build the container image */            
                    def dockerImage = docker.build("my-image:${env.BUILD_ID}")
                      
                    /* Push the container to the custom Registry */
                    dockerImage.push()
                      
                    
                    /* Remove docker image*/
                    sh 'docker rmi -f my-image:${env.BUILD_ID}'   
               }
            } 
        }


    }
}