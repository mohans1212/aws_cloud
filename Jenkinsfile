pipeline {
    agent any

    environment {
        IMAGE_NAME = 'mysite:v2'
        CONTAINER_NAME = 'Profile2'
    }

    stages {
        stage('Checkout'){
            steps{
                git url:'https://github.com/mohans1212/aws_cloud'
            }
        }
        stage('BuildCont'){
            steps{
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }
        stage('Print Env Vars') {
                steps {
                    sh 'printenv'
                }
            }
    }
}
