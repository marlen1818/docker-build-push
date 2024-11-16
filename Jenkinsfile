pipeline{
    agent any
    environment {
        DOCKERHUB_USERNAME = "feery18"
        DOCKER_IMAGE_NAME = "my-app"
        DOCKER_TAG = "V.${BUILD_NUMBER}"
        IMAGE_URL = "${DOCKERHUB_USERNAME}/${DOCKER_IMAGE_NAME}:${DOCKER_TAG}"
    }
    stages{
        stage('imageBuild'){
            steps{
                sh "docker build -t ${IMAGE_URL} ."
            }
        }
        stage('dockerAuthPush'){
            steps{
                script{
                    withCredentials([usernamePassword(credentialsId: 'docker-login', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh "echo $PASS | docker login -u ${USER} --password-stdin"
                        
                        // some block
                        sh "docker push ${IMAGE_URL}"
                    }
                }
            }
        }        
        stage('removeImage'){
            steps{       
                sh "docker image rm ${IMAGE_URL}"         
                echo "Image removed succesfully"
            }
        }        
    }
    post{
        success{
            echo 'Docker Image Built and Pushed '
            echo 'Triggering CD pipeline '

        }
        failure{
            echo 'Something went wrong'
        }
        cleanup{
            cleanWs()
        }
    }
}









