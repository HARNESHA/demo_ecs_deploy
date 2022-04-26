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
                npm install
                npm install -g pm2'''
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
