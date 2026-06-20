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
        stage('Build') {
            steps {
                sh 'python3 app.py'
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
