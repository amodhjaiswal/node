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

        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Ensure SonarQube Scanner is configured correctly
                // Replace placeholder values with actual configuration
                sh '''
                ${tool 'SonarQube Scanner'}/bin/sonar-scanner \
                -Dsonar.projectKey=my-project \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://your-sonarqube-server:9000 \
                -Dsonar.login=your-retail-token
                '''
            }
        }

        stage('Deploy React App') {
            steps {
                sh '''
                rsync -avz -e "ssh -i /home/jenkins/.ssh/authorized_keys" \
                ${WORKSPACE}/build/ ubuntu@65.0.94.218:/var/www/html
                '''
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            cleanWs() // Clean up workspace
        }
    }
}
