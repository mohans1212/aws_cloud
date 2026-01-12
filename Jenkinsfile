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
                    docker tag ${BUILD_TAG}:${BUILD_ID} mohancloud12/one:${BUILD_TAG}
                    docker push mohancloud12/one:${BUILD_TAG}
                    docker logout
                '''
                }
             }
        }
        stage('Update file') {
            steps {
                sh '''

                    IMAGE="mohancloud12/one:${BUILD_TAG}"
                    yq e ".spec.template.spec.containers[0].image = \\"${IMAGE}\\"" -i kubernetes/deployment.yml
                    cat kubernetes/deployment.yml
                '''
            }
        }
        stage('Commit and Push Changes') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'git-creds',
                        usernameVariable: 'GIT_USER',
                        passwordVariable: 'GIT_PASS'
                    )
                ]) {
                    sh '''
                      git config user.name "jenkins"
                      git config user.email "jenkins@ci"

                      git add kubernetes/deployment.yml
                      git commit -m "test" || echo "No changes to commit"

                      git push https://${GIT_USER}:${GIT_PASS}@github.com/mohans1212/aws_cloud.git dev
                    '''
            }
         
          }
        }
    //  stage('Kubernetes Deployment') {
    //         steps {
    //             sh "kubectl set image deployment/metric-deploy cont=mohancloud12/one:${BUILD_TAG}"
    //         }
    //    }   
  }   
}
