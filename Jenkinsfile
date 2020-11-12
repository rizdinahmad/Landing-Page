env.DOCKER_REGISTRY = '25092018'
env.DOCKER_IMAGE_NAME = 'stglandingpage'
pipeline {
    agent any
    stages {
        stage ('build') {    
            steps {
               sh "docker build --build-arg APP_NAME=$DOCKER_IMAGE_NAME -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$BUILD_NUMBER ."
            }
        }
        stage ('push') {
            steps {                
                sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$BUILD_NUMBER"
            }
        }
        stage ('deploy') {
            steps {
                sh('sed -i "s/@/$BUILD_NUMBER/g" staging-landingpage.yml')
                sh('kubectl apply -f staging-landingpage.yml')
            }
        }
        stage ('Remove Image') {
            steps {
                sh "docker rmi $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$BUILD_NUMBER"
            }
        }
        stage('Ingress') {
            steps {
                sh "kubectl get ingress -n staging"
            }
        }
    }
}
