pipeline {
    agent any

    stages {
        stage('Clone React App') {
            steps {
                git 'https://github.com/amodhjaiswal/node.git'
            }
        }
        stage('Build React App') {
            steps {
                sh 'npm install'
             
            }
        }

  stage('Sonar-scanner') {
            steps {
                //figure out :) will let you know but first try
             
            }
        }


 stage('Build React App') {
            steps {
                
                sh 'npm run build'
            }
        }
        
        stage('Deploy React App') {
            steps {
               
                //sh 'scp -i /home/jenkins/.ssh/authorized_keys /var/lib/jenkins/workspace/node/build ubuntu@3.106.222.255:/home/ubuntu'
                sh 'rsync -avz -e "ssh -i /home/jenkins/.ssh/authorized_keys " /var/lib/jenkins/workspace/node/build ubuntu@3.106.222.255:/var/www/html'
            }
        }
       
        
       
    }
}

