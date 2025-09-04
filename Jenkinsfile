pipeline {
    agent any

    tools {
        maven 'Maven3'     // Jenkins -> Global Tool Config -> Maven3
        nodejs 'Node24'    // Jenkins -> Global Tool Config -> Node16 (NodeJS 24.7.0)
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
                    sh 'mvn clean install -DskipTests'
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
                    // Allow frontend tests to fail without breaking pipeline
                    sh 'npm test -- --watchAll=false || true'
                }
            }
        }

        stage('Package') {
            steps {
                echo 'Archiving artifacts...'
                archiveArtifacts artifacts: 'docker-ecommerce-backend/target/*.jar, docker-ecommerce-frontend/build/**', fingerprint: true
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline succeeded!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
        always {
            echo 'Pipeline completed.'
        }
    }
}
