pipeline {
    agent any

    environment {
        IMAGE_NAME = "${BUILD_TAG}"
        CONTAINER_NAME = "Profile"
    }

    stages {
        stage('Checkout'){
            steps{
                git branch:'main',url:'https://github.com/mohans1212/aws_cloud'
            }
        }
        stage('BuildCont'){
            steps{
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }
        stage('deploy'){
            steps{
                sh "docker run -d --name ${CONTAINER_NAME} -p 8081:80 ${IMAGE_NAME}"
            }
        }
        stage('Print Env Vars') {
                steps {
                    sh 'printenv | sort'
                }
            }
    }
}
