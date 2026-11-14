












// pipeline{
//     agent any 
//     environment{
//         DOCKERHUB_USERNAME = "yemmie69"
//         IMAGE_NAME = "${DOCKERHUB_USERNAME}/nodejs-cicd-app"
//         APP_PORT = "3000"
//         CONTAINER_NAME = "nodejs-apps"
//     }
//     stages{
//         stage("Checkout source code"){
//             steps{
//                 checkout scm
//             }
//         }

//         stage("Install dependencies"){
//             steps{
//                 sh "npm ci"
//             }
//         }

//          stage("Build docker image"){
//             steps{
//                 sh """
//                 docker build \
//                 -t ${IMAGE_NAME}:${BUILD_NUMBER} \
//                 -t ${IMAGE_NAME}:latest .
//                 """
//             }
//         }
//         stage("Push to docker hub"){
//             steps{
//                 withCredentials([
//                     usernamePassword(
//                         credentialsId: 'docker-credentials',
//                         usernameVariable: 'DOCKERHUB_USERNAME',
//                         passwordVariable: 'DOCKER_TOKEN'
//                     )
//                 ]){
//                     sh """
//                     echo "$DOCKER_TOKEN" | docker login -u "$DOCKERHUB_USERNAME" --password-stdin
//                     docker push ${IMAGE_NAME}:${BUILD_NUMBER}
//                     docker push ${IMAGE_NAME}:latest
//                     docker logout
//                     """
//                 }
//             }
//         }
//     }
// }



pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = "yemmie69"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}/nodejs-cicd-app"
        APP_PORT = "3000"
        CONTAINER_NAME = "nodejs-apps"
    }

    stages {

        stage('Checkout Source Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm ci'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                    docker build \
                    -t ${IMAGE_NAME}:${BUILD_NUMBER} \
                    -t ${IMAGE_NAME}:latest .
                """
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'docker-credentials',
                        usernameVariable: 'DOCKERHUB_USER',
                        passwordVariable: 'DOCKER_TOKEN'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_TOKEN" | docker login \
                        -u "$DOCKERHUB_USER" \
                        --password-stdin

                        docker push "$IMAGE_NAME:$BUILD_NUMBER"
                        docker push "$IMAGE_NAME:latest"

                        docker logout
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }

        success {
            echo 'Docker image successfully pushed to Docker Hub!'
        }

        failure {
            echo 'Pipeline failed. Check the Jenkins console output.'
        }
    }
}
