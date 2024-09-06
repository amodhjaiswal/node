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
                sh 'rsync -avz -e "ssh -i /home/jenkins/.ssh/id_rsa " /var/lib/jenkins/workspace/mynewsonar/build/ ubuntu@65.0.94.218:/var/www/html '
            }
        }
    }

    post {
        always {
            cleanWs() // Clean up the workspace after the build
        }
    }
}
