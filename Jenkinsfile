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

        stage('Sonar-scanner') {
            steps {
                // Add SonarQube scanner commands here
                // Example:
                // sh "${tool 'SonarQube Scanner'}/bin/sonar-scanner \
                // -Dsonar.projectKey=my-project \
                // -Dsonar.sources=. \
                // -Dsonar.host.url=http:65.0.94.218:9000 \
                // -Dsonar.login=retail-token"
            }
        }

        stage('Deploy React App') {
            steps {
                sh 'rsync -avz -e "ssh -i /home/jenkins/.ssh/authorized_keys" /var/lib/jenkins/workspace/node/build ubuntu@3.106.222.255:/var/www/html'
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
