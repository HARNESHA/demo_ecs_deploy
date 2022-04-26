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
               sh '''npm install -g npm@latest
                npm install pm2 -g
                npm install'''
            }
        }

        stage('check') {
            steps {
              sh '''node --version
                    npm --version
                    pm2 --version'''
            }
        }
    }     
}
