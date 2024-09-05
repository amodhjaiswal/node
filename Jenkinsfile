pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('newsonar') // Replace 'sonar-token-id' with your actual SonarQube token ID in Jenkins credentials
    }

    stages {
        stage('Clone React App') {
            steps {
                git 'https://github.com/saba-2002/react-a-saba.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('SonarQube Scan') {
            steps {
                script {
                    withSonarQubeEnv('SonarQube') { // Replace 'SonarQube' with the name of your SonarQube installation in Jenkins
                        sh '''
                        sonar-scanner \
                            -Dsonar.projectKey=sonarscancode \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://65.0.94.218:9000 \
                            -Dsonar.login=
                        '''
                    }
                }
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy React App') {
            steps {
                sh '''
                # Define the source and target directories
                SOURCE_DIR="/var/lib/jenkins/workspace/react-a-saba/build/"
                TARGET_DIR="/var/www/html/"

                # Copy files from source to target
                cp -r ${SOURCE_DIR}* ${TARGET_DIR}
                '''
            }
        }
    }

    post {
        always {
            cleanWs() // Clean up the workspace after the build
        }
    }
}
