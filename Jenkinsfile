pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                /*sh"sudo javac app.java"
                sh "java HelloWorld"*/
                sh "sudo docker build -t my-java-app ."
               // sh "sudo docker run --rm --name my-running-app my-java-app"

            }
            
        }
        stage('Test') {
            steps {
                echo 'Testing'
                sh 'make check || true' 
                junit '**/target/*.xml' 
            }
        }

        stage('Deploy to Kubernetes Cluster') {
            when {
              expression {
                currentBuild.result == null || currentBuild.result == 'SUCCESS' 
              }
            }
            steps {
                //sh  'sudo kubectl run javapp --image=my-java-app --port=80'
                sh 'sudo chmod +rwx /home/boras/.minikube/ca.crt'
                sh 'sudo chmod +rwx /home/boras/.minikube/profiles/minikube/client.crt'
                sh 'sudo chmod +rwx /home/boras/.minikube/profiles/minikube/client.key'
                kubernetesDeploy(configs:"file.yaml", kubeconfigId: "../.kube/config")
                //sh "sudo kubectl create -f file.yaml"
                        
            }
        }

    }
}