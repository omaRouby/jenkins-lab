@Library('shared-library')_
pipeline {
    agent any
    
    environment {
        dockerHubCredentialsID    = 'omarrouby'                 // DockerHub credentials ID.
        imageName                 = 'omarrouby/lab-app-python' // DockerHub repo/image name.
     k8sCredentialsID             = 'kubeconfig'              // KubeConfig credentials ID.    
    }
    
    stages {       
       
        
        stage('build and Push image to Docker hub') {
            steps {
                script {
                  
                          buildPushImage("${dockerHubCredentialsID}", "${imageName}")
                      
                }
            }
        }

        stage('Edit new image in the deployment') {
            steps {
                script { 
                          editImage("${imageName}")
                }
            }
        }

        stage('Deploy on kubernetes Cluster') {
            steps {
                script {  
                         deployOnKubernetes("${k8sCredentialsID}", "${imageName}")
                }
            }
        }
    }

    post {
        always {
            echo "${JOB_NAME}-${BUILD_NUMBER} pipeline always succeeded"
        }
        success {
            echo "${JOB_NAME}-${BUILD_NUMBER} pipeline succeeded"
        }
        failure {
            echo "${JOB_NAME}-${BUILD_NUMBER} pipeline failed"
        }
    }
}
