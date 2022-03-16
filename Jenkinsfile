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
        stage('List pods') {
            withKubeConfig([credentialsId: '<credential-id>',
                    caCertificate: '<ca-certificate>',
                    serverUrl: '<api-server-address>',
                    contextName: '<context-name>',
                    clusterName: '<cluster-name>',
                    namespace: '<namespace>'
                    ]) {
                sh 'kubectl get pods'
                }
        }

        stage('Deploy to Kubernetes Cluster') {
            when {
              expression {
                currentBuild.result == null || currentBuild.result == 'SUCCESS' 
              }
            }
            steps {
                //sh  'kubectl run javapp --image=my-java-app --port=80'           
                kubernetesDeploy(configs:"file.yaml", kubeconfigId: "mykubeconfig")
                //sh "kubectl create -f file.yaml"
                        
            }
        }

    }
}