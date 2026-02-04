pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat '''
                py -m py_compile sources/add2vals.py sources/calc.py
                '''
            }
        }

        stage('Test') {
            steps {
                bat '''
                mkdir test-reports
                py -m pytest --verbose --junit-xml=test-reports/results.xml sources/test_calc.py
                '''
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }

        stage('Deliver') {
            steps {
                bat '''
                py -m PyInstaller --onefile sources/add2vals.py
                '''
            }
            post {
                success {
                    archiveArtifacts artifacts: 'dist/*.exe'
                }
            }
        }
    }
}
