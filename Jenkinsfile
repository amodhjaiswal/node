pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'SonarQube' // The name of your SonarQube server configuration in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the pull request code
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                // Perform the build. Adjust according to your build tool
                // For example, for a Maven project:
                sh 'mvn clean install'
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                // Perform SonarQube analysis using the SonarQube scanner
                withSonarQubeEnv(SONARQUBE_SERVER) {
                    sh 'sonar-scanner \
                        -Dsonar.projectKey=mynewprojforgit \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://65.0.94.218:9000 \
                        -Dsonar.login=sqp_81eaf243d092b37675a466efcd0abee3233b6d81'
                }
            }
        }
        
        stage('Quality Gate') {
            steps {
                // Wait for SonarQube to analyze the code and get the quality gate result
                script {
                    def qualityGate = waitForQualityGate()
                    if (qualityGate.status != 'OK') {
                        error "SonarQube Quality Gate failed: ${qualityGate.status}"
                    }
                }
            }
        }
        
        stage('Merge to Main') {
            when {
                expression {
                    // Only run if the branch is a pull request
                    return env.CHANGE_ID
                }
            }
            steps {
                script {
                    def mergeCmd = """
                    git config user.email "jenkins@example.com"
                    git config user.name "Jenkins"
                    git checkout main
                    git pull origin main
                    git merge ${env.CHANGE_BRANCH}
                    git push origin main
                    """
                    sh mergeCmd
                }
            }
        }
    }

    post {
        always {
            // Clean up the workspace after the build
            cleanWs()
        }
        success {
            echo 'Build and analysis completed successfully.'
        }
        failure {
            echo 'Build or analysis failed.'
        }
    }
}
