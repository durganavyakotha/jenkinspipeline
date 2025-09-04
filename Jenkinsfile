pipeline {
    agent any

    tools {
        maven 'Maven3'     // Make sure Maven is configured in Jenkins
        nodejs 'Node16'    // Make sure NodeJS is configured in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/durganavyakotha/jenkinspipeline.git'
            }
        }

        stage('Backend Build') {
            steps {
                dir('docker-ecommerce-backend') {
                    sh 'mvn clean install'
                }
            }
        }

        stage('Frontend Build') {
            steps {
                dir('docker-ecommerce-frontend') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running unit tests...'
                dir('docker-ecommerce-backend') {
                    sh 'mvn test'
                }
                dir('docker-ecommerce-frontend') {
                    sh 'npm test -- --watchAll=false || true'
                }
            }
        }

        stage('Package') {
            steps {
                echo 'Archiving artifacts...'
                archiveArtifacts artifacts: '**/target/*.jar, docker-ecommerce-frontend/build/**', fingerprint: true
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
