env.DOCKER_REGISTRY = 'rizdinahmad'
env.DOCKER_IMAGE_NAME = 'dev-landingpage'
pipeline {
    agent any
    stages {
        stage('Git Pull from Github') {
            steps {
                git credentialsId: 'githubpass', url: 'https://github.com/rizdinahmad/landing-page.git'
            }
        }
        stage('Build Image') {
            steps {
                sh "docker build --build-arg APP_NAME=$DOCKER_IMAGE_NAME -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$BUILD_NUMBER ."
                }
        }
        stage('Push Image to DockerHub') {
            steps {                
                sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$BUILD_NUMBER"
              
                sh('sed -i "s/landing-page:tag/landing-page:$BUILD_NUMBER/g" staging-landingpage.yml')
                
                sh('kubectl apply -f staging-landingpage.yml -n staging')
                
                sh "docker rmi $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$BUILD_NUMBER"
                
                sh('kubectl get ingress -n staging')
                }
        }
    }  
}
