env.DOCKER_REGISTRY = '25092018'
env.DOCKER_IMAGE_NAME = 'stglandingpage'
pipeline {
    agent any
    stages {
        stage ('Start') {    
            steps {
                 sh('sed -i "s/tag/$BUILD_NUMBER/g" index.html')
            }}
        stage ('Docker Build'){
            steps {
                sh "docker build --build-arg APP_NAME=$DOCKER_IMAGE_NAME -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$BUILD_NUMBER ."
            }}
        stage ('Docker Push') {
            steps {                
                sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$BUILD_NUMBER"
            }}
        stage ('tagging') {
            steps {
                sh('sed -i "s/tag/$BUILD_NUMBER/g" staging-landingpage.yml')
            }}
        stage('locate namespace') {
            steps {
                sh('sed -i "s/staging/staging/g" staging-landingpage.yml')
            }}
        stage('add domain') {
            steps {
                sh('sed -i "s/stglandingpage.rizdin.online/stglandingpage.rizdin.online/g" staging-landingpage.yml')
            }}
        stage('Ingress') {
            steps {
                sh('kubectl apply -f staging-landingpage.yml')
                sh "docker rmi $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$BUILD_NUMBER"
            }}
        stage('add domain') {
            steps {
                sh "kubectl get ingress -n staging"
            }}
    }
}
