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
                dir('code/frontend'){sh 'npm install'}
                dir('code/backend'){sh 'npm install'}
            }
        }
        stage('Build') {
            agent {
                docker { image 'node:alpine' }
            }
            steps {
		sh 'docker-compose -f docker-compose.yml build'
                sh 'npm run build'  
            }
        }
        stage('Static Analysis') {
            agent {
                docker { image 'node:alpine' }
            }
            steps {
                echo 'Analyze' 
            }
        }
        stage('Unit Test') {
            agent {
                docker { image 'node:alpine' }
            }
            steps {
                echo 'Test'
            }
        }
        stage('e2e Test') {
            steps {             
                sh 'docker-compose -f docker-compose-e2e.yml up -d frontend backend'
            }
            post {
                always {
                    sh 'docker-compose -f docker-compose-e2e.yml down --rmi=all -v'
                }
            }
        }
        stage('Deploy') {
            steps {                
                sh 'docker-compose -f docker-compose.yml up -d'
            }
        }
    }
}
