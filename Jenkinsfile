pipeline {
    agent {
        docker {
            image 'josedom24/debian-npm'
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

        stage('Install npm') {
            steps {
                script {
                    sh 'apt update -y && apt install npm -y'
                }
            }
        }

        stage('Actualizar npm') {
            steps {
                script {
                    sh 'curl -I https://registry.npmjs.org/'
                }
            }
        } 

        stage('Install Surge') {
            steps {
                script {
                    sh 'npm install -g surge'
                }
            }
        }
       
        stage('Install Pip') {
            steps {
                script {
                    sh 'apt update -y && apt install pip default-jre -y'
                }
            }
        }
       
        stage('Install Dependencies') {
            steps {
                script {
                    sh 'pip install html5validator'
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
