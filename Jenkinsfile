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
        
        stage('Deploy React App') {
            steps {
                // Using rsync for deployment
                sh 'cp -r /var/lib/jenkins/workspace/mysonargithub/build/* /var/html/''
            }
        }


        
    }
}

