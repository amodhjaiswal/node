pipeline {
    agent any

    stages {
        stage('Clone React App') {
            steps {
                git 'https://github.com/amodhjaiswal/node.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Placeholder for SonarQube scanner configuration
                // Ensure you have SonarQube plugin configured in Jenkins
                // Example command:
                // sh 'sonar-scanner -Dsonar.projectKey=my-project -Dsonar.sources=src'
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }
        
        stage('Deploy React App') {
            steps {
                // Using rsync for deployment
                sh 'rsync -avz -e "ssh -i /home/jenkins/.ssh/authorized_keys" /var/lib/jenkins/workspace/node/build/ ubuntu@3.106.222.255:/var/www/html'
            }
        }
    }
}

