pipeline {
    agent any

    environment {
        MAVEN_HOME = tool name: 'Maven 3.8.5', type: 'maven'
        SONARQUBE = 'SonarQube' // This is the name of your SonarQube server in Jenkins configuration
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from Git repository
                git url: 'https://github.com/your-username/your-repo.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                script {
                    // Execute Maven build
                    sh "${MAVEN_HOME}/bin/mvn clean install"
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Run SonarQube analysis
                    sh """
                        ${MAVEN_HOME}/bin/mvn sonar:sonar \
                        -Dsonar.projectKey=your-project-key \
                        -Dsonar.host.url=http://your-sonarqube-server:9000 \
                        -Dsonar.login=your-sonarqube-token
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    // Check SonarQube quality gate status
                    def qg = waitForQualityGate()
                    if (qg.status != 'OK') {
                        error "Pipeline aborted due to quality gate failure: ${qg.status}"
                    }
                }
            }
        }

        stage('Merge to Main') {
            when {
                branch 'feature/*'
            }
            steps {
                script {
                    // Merge to main branch
                    sh 'git checkout main'
                    sh 'git merge feature/your-feature-branch'
                    sh 'git push origin main'
                }
            }
        }
    }

    post {
        always {
            // Clean up workspace
            cleanWs()
        }
    }
}
