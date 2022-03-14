pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh"sudo javac app.java"
                sh "java HelloWorld"
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


    }
}