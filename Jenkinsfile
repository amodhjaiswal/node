pipeline {
    agent any

    environment {
        // Define your SonarQube token here using Jenkins credentials plugin
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
                    // Run SonarQube scan using the installed SonarQube Scanner
                    withSonarQubeEnv('SonarQube') { // Replace 'SonarQube' with the name of your SonarQube installation in Jenkins
                        sh '''
                        sonar-scanner \
                            -Dsonar.projectKey=sonarscancode \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://65.0.94.218:9000 \
                            -Dsonar.login=sqp_76fa3cbe4ef53fbb9ed8081d2ee65d2f1e968b7b
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
                rsync -avz -e "ssh -i /home/jenkins/.ssh/id_rsa" \
                    /var/lib/jenkins/workspace/react-a-saba/build/ \
                    ubuntu@3.106.222.255:/var/www/html
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
