pipeline {
    agent any

    options {
        timestamps()
        buildDiscarder(logRotator(numToKeepStr: '10'))
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Code already present in workspace (local demo).'
            }
        }
        stage('Use Secret Safely') {
            steps {
                withCredentials([string(credentialsId: 'demo-secret-text', variable: 'MY_SECRET')]) {
                    sh 'echo "The secret length is: ${#MY_SECRET}"'
                }
            }
        }
        stage('Install') {
            steps {
                sh 'python3 -m pip install --break-system-packages pytest || true'
            }
        }
        stage('Test') {
            steps {
                sh 'python3 -m pytest test_app.py -v'
            }
        }
        stage('Isolated Python Test') {
            agent {
                docker { image 'python:3.11-slim' }
            }
            steps {
                sh 'pip install pytest && pytest --version'
            }
        }
        stage('Parallel Checks') {
            parallel {
                stage('Lint') {
                    steps { sh 'echo "Pretend linting..." && sleep 2' }
                }
                stage('Security Scan') {
                    steps { sh 'echo "Pretend security scan..." && sleep 2' }
                }
            }
        }
        stage('Build') {
            steps {
                sh 'python3 app.py'
            }
        }
         stage('Docker Build') {
            steps {
                sh 'docker build -t demo-pipeline:${BUILD_NUMBER} .'
            }
        }
        stage('Docker Run') {
            steps {
                sh 'docker run --rm demo-pipeline:${BUILD_NUMBER}'
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed — check the Test stage first.'
        }
        always {
            echo 'This always runs, pass or fail (Q46: post block).'
        }
    }
}
