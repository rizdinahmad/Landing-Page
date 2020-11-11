env.DOCKER_REGISTRY = '25092018'
env.DOCKER_IMAGE_NAME = 'stglandingpage'
pipeline {
    agent any
    stages {
        stage('Deliver for development') {
            when {
                branch 'dev' 
            }
            steps {
                git credentialsId: 'githubpass', url: 'https://github.com/rizdinahmad/landing-page.git'
            }
            steps {
                sh "docker build --build-arg APP_NAME=$DOCKER_IMAGE_NAME -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$BUILD_NUMBER ."
            }
            steps {                
                sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$BUILD_NUMBER"
                sh('kubectl delete -f staging-landingpage.yml')
                sh('kubectl apply -f staging-landingpage.yml')
                sh "docker rmi $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$BUILD_NUMBER"
                sh "kubectl get ingress -n staging"
            }
        }
     }
}
