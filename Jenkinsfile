pipeline {
    agent {
        label 'master'
    }
    stages {
        stage('Prepare') {
            agent {
                docker { image 'node:alpine' }
            }
            steps {
                dir('code/backend'){
                sh 'npm install'
                }
                dir('code/frontend'){
                sh 'npm install'
                }
            }
        }
        stage('Build') {
            agent {
                docker { image 'node:alpine' }
            }
            steps {
                dir('code/backend'){
                sh 'npm run build'
                }
                   
            }
        }
        stage('Static Analysis') {
            agent {
                docker { image 'node:alpine' }
            }
            steps {
                dir('code/backend'){
                sh 'npm run lint'
                }
            }
        }
        stage('Unit Test') {
            agent {
                docker { image 'node:alpine' }
            }
            steps {
                dir('code/backend'){
                sh 'npm run test'
                }
            }
        }
        stage('e2e Test') {
            steps {             
                dir('ci/code'){
                sh 'docker-compose -f docker-compose-e2e.yml up -d frontend backend'
                }
            }
            post {
                always {
                   dir('ci/code'){
                    sh 'docker-compose -f docker-compose-e2e.yml down --rmi=all -v'                }
                }
            }
        }
        stage('Deploy') {
            steps {                
                   dir('ci/code'){
                       sh 'docker-compose -f docker-compose.yml'
                   }
            }
        }
    }
}
