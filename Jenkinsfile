pipeline {
    agent any

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
                    // Run SonarQube scan
                    sh """
                    sonar-scanner \
                        -Dsonar.projectKey=sonarscancode \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://65.0.94.218:9000 \
                        -Dsonar.login=sqp_76fa3cbe4ef53fbb9ed8081d2ee65d2f1e968b7b
                    """
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
                // Use rsync for deployment
                sh '''
                rsync -avz -e "ssh -i /home/jenkins/.ssh/id_rsa" \
                    /var/lib/jenkins/workspace/react-a-saba/build/ \
                    ubuntu@3.106.222.255:/var/www/html
                '''
            }
        }
    }

    environment {
        // Use Jenkins credentials for SonarQube token
        SONAR_TOKEN = credentials('sonar-token-id') // Replace 'sonar-token-id' with the actual credentials ID
    }

    post {
        always {
            // Clean up workspace or other steps you want to run after the pipeline
            cleanWs()
        }
    }
}
