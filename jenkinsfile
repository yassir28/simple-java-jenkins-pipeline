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
            }
            
        }
    }
}
