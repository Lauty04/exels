pipeline {
    agent {
        node {
            label 'nodiono'
        }
    }
    environment {
        SSH_PASSWD = '12345'
        GIT_CREDENTIALS = credentials('tokengit')
        GIT_USERNAME = 'Lauty04'
        GIT_PASSWORD = 'Lauti2004#'
        
        
    }
    stages {
        stage('Connect to Docker Node') {
            steps {
                script {
                    sh 'pwd';
                    sh 'chmod +x /opt/jenkins/workspace/nodonio/python-diff.py';
                    sh './python-diff.py old.xlsx new.xlsx'
                    echo "Python script completed..."
                    sh 'bash ';
                    sh "sshpass -p $SSH_PASSWD scp -o StrictHostKeyChecking=no /opt/jenkins/workspace/nodonio/meta-script.sh lautaro@172.17.0.4:/home/lautaro/"
                    // sh "sshpass -p $SSH_PASSWD ssh lautaro@172.17.0.4 'sudo su'" 
                    sh 'sshpass -p $SSH_PASSWD ssh lautaro@172.17.0.4 "chmod +x /home/lautaro/meta-script.sh "'
                    sh 'whoami'
                    sh "sshpass -p $SSH_PASSWD ssh lautaro@172.17.0.4 'bash /home/lautaro/meta-script.sh'"  
                    
                }

            }
            
        }
        stage('Convert MD to PDF') {
            steps {
                script {
                    sh 'pandoc logs.md --pdf-engine=xelatex -o logs.pdf'
                }
            }
        }
        
        stage('Push to GitHub') {
            steps {
                script {
                     withCredentials([string(credentialsId: 'tokengit', variable: 'GIT_CREDENTIALS')]) {
                        sh 'git config --global user.email "lalor07@gmail.com"'
                        sh 'git config --global user.name "Lauty04"'
                        sh 'git add logs.pdf'
                        sh 'git commit -m "Actualizar archivo desde Jenkins"'
                        sh 'git tag -a some_tag -m "Jenkins"'
                        sh 'git push https://${GIT_CREDENTIALS}@github.com/Lauty04/exels.git --tags'
                        sh 'git push https://${GIT_CREDENTIALS}@github.com/Lauty04/exels.git main'
                    }
                }
            }
        }


    }
    
}
