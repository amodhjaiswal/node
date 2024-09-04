pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'SonarQube' // The name of your SonarQube server configuration in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                checkout scm
            }
        }
        
        stage('Verify Project Structure') {
            steps {
                script {
                    // Verify that the POM file exists
                    def pomFile = fileExists 'pom.xml'
                    if (!pomFile) {
                        error "Maven POM file not found in the workspace."
                    }
                }
            }
        }
        
        stage('Build') {
            steps {
                script {
                    def mvnHome = tool name: 'Maven 3', type: 'maven'
                    if (mvnHome == null) {
                        error "Maven installation not found. Please configure Maven in Jenkins."
                    }
                    
                    // If your pom.xml is not in the root directory, navigate to the correct directory
                    // sh "cd path/to/maven/project && ${mvnHome}/bin/mvn clean install"
                    sh "${mvnHome}/bin/mvn clean install"
                }
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
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
