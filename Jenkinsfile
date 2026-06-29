pipeline {
    agent any

    environment {
        DOCKER_CRED = credentials('DockerHub') 
    }

    stages {
        stage('fetch-code') {
            steps {
                git branch: 'main', url: 'https://github.com/Shreek195/jenkins-demo.git'
            }
        }

        stage('build-image') {
            steps {
                sh "docker build -t dheeraj310702/jenkis-app:${BUILD_NUMBER} ."
            }
        }

        stage('push-image') {
            steps {
                sh 'echo "$DOCKER_CRED_PSW" | docker login -u dheeraj310702 --password-stdin'
                sh "docker push dheeraj310702/jenkis-app:${BUILD_NUMBER}"
            }
        }

        // stage('verify-build') {
        //     steps {
        //         sh 'docker logout'
        //         sh 'echo "$DOCKER_CRED_PSW" | docker login -u dheeraj310702 --password-stdin'
        //         sh "docker push dheeraj310702/jenkis-app:${BUILD_NUMBER}"
        //     }
        // }

        
        stage('update-minikube') {
            steps {
                sh "kubectl set image deployment/pd1 jenkis-demo=dheeraj310702/jenkis-app:${BUILD_NUMBER}"
            }
        }
    }
    
    // post {
    //     always {
    //         sh 'docker logout'
    //     }
    // }
}
