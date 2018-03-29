pipeline {
 def customImage = docker.build("my-image:${env.BUILD_ID}")
 agent {
   docker { label 'docker'}
 }
 stages {
  stage('Build') {
   steps {
    sh "docker build -t mitchybgood/dogs:${GIT_SHA} ."
  }
 }
 stage('Publish') {
   steps {
     sh "docker push mitchybgood/dogs:${GIT_SHA}"
   }
 }
}
