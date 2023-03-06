pipeline {
   agent {
      label 'adiv'}

   stages {

    stage('Cloning Git') {

      steps
        {
        /* Let's make sure we have the repository cloned to our workspace */
        checkout scmGit(branches: [[name: '*/email-notification']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/adivaida/trainmefordevsecops']])        }
    }
    stage('SAST'){
      steps{
        sh 'echo SAST stage'
       }
    }


    stage('Build-and-Tag') {
      steps{
         script {
            app = docker.build("adiv/snack:${env.BUILD_ID}") 
          }
    }

    stage('Post-to-dockerhub') {
     steps {
        sh 'echo post to dockerhub repo'
     }
    }

    stage('SECURITY-IMAGE-SCANNER'){
      steps {
        sh 'echo scan image for security'
     }
    }

    stage('Pull-image-server') {
      steps {
         sh 'echo pulling image ...'
       }
      }

    stage('DAST') {
      steps  {
         sh 'echo dast scan for security'
        }
    }
 }
}
