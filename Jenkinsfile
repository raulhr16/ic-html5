pipeline {
    agent {
        docker {
            image 'raulhr16/surge:v1'
            args '-u root:root'
        }
    }
   
    environment {
        TOKEN = credentials('SURGE_TOKEN')
    }
   
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/raulhr16/ic-html5'
            }
        }
       
        stage('Install Pip') {
            steps {
                script {
                    sh 'apt update -y && apt install pip python3-venv default-jre -y'
                }
            }
        }
       
        stage('Install Dependencies') {
            steps {
                script {
                    sh '''
                        python3 -m venv venv
                        . venv/bin/activate
                        pip install --no-cache-dir html5validator
                    '''
                }
            }
        }
       
        stage('Test HTML') {
            steps {
                script {
                    sh 'html5validator --root _build/'
                }
            }
        }
       
        stage('Deploy') {
            steps {
                script {
                    sh 'surge ./_build/ raulhr.surge.sh --token $TOKEN'
                }
            }
        }
    }
}
