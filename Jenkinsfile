pipeline {
   agent {
      label 'adiv'
      }

   stages {

        stage('Cloning Git') {

        steps
            {
            /* Let's make sure we have the repository cloned to our workspace */
            checkout scmGit(branches: [[name: '*/email-notification']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/adivaida/trainmefordevsecops']])
            }
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
        }
        stage('Post to Docker Hub  ') {
            steps {
                script {
                docker.withRegistry('https://registry.hub.docker.com','dockerhub') {
                    app.push("${env.BUILD_ID}")
                    }
                }
                }
        }
        stage('SECURITY-IMAGE-SCANNER'){
        steps {
            sh 'echo scan image for security'
        }
        }

        stage('Pull image Server  ') {
            steps {
                     sh 'docker-compose down'
                     sh 'docker-compose up'
                
                }
        }

        stage('DAST') {
        steps  {
            sh 'echo dast scan for security'
            }
        }
    }
}
