pipeline {
    agent any

    environment {
        IMAGE_NAME = "${BUILD_TAG}:${BUILD_ID}"
        CONTAINER_NAME = "Profile"
        REG_CRED_ID = "dockerpass" 
    }

    stages {
        stage('Checkout'){
            steps{
                git branch:'dev',url:'https://github.com/mohans1212/aws_cloud'
            }
        }
        stage('BuildCont'){
            steps{
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }
        stage('deploy'){
            steps{
                sh "docker stop ${CONTAINER_NAME} || true"
                sh "docker rm ${CONTAINER_NAME} || true"
                sh "docker run -d --name ${CONTAINER_NAME} -p 8081:80 ${IMAGE_NAME}"
            }
        }
        stage('Push Image') {
        steps {
        withCredentials([usernamePassword(credentialsId: REG_CRED_ID, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          sh '''
            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
            docker push ${IMAGE_NAME}
            docker logout
          '''
        }
      }
    }
    }
}
