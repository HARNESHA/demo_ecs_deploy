pipeline {
    agent any


    tools {nodejs "node"}

    stages {

        stage('info') {
            steps {
               sh '''id
                    pwd
                    ls -alh
                    '''
            }
        }

        stage('Build') {
            steps {
               sh '''
                rm -rf node_modules 
                ls -alh
                npm install
                '''
            }
        }

        stage('check') {
            steps {
              sh '''node --version
                    npm --version
                    pm2 --version'''
            }
        }

        stage('deploy') {
            steps {
              sh '''
                ls -alh
                pm2 start server.js
                netstat -ntlp
              '''
            }
        }
    }     
}
