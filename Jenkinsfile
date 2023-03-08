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
        stage('SAST') {
            environment {
                SONAR_SCANNER_VERSION = '4.7.0.2747'
                SONAR_TOKEN=credentials('adi-sonar')
                SONAR_SCANNER_HOME = "$HOME/.sonar/sonar-scanner-$SONAR_SCANNER_VERSION-linux"
                PATH = "$SONAR_SCANNER_HOME/bin:$PATH"
                SONAR_SCANNER_OPTS = '-server'
            }
            steps {
                sh 'curl --create-dirs -sSLo $HOME/.sonar/sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-$SONAR_SCANNER_VERSION-linux.zip'
                sh 'unzip -o $HOME/.sonar/sonar-scanner.zip -d $HOME/.sonar/'
                sh '''sonar-scanner \
                    -Dsonar.organization=adivaida \
                    -Dsonar.projectKey=trainmefordevsecops \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=https://sonarcloud.io'''
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
