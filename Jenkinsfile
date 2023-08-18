pipeline {
    agent { label 'Agent node' }
    
    stages{
        stage('Checkout'){
            steps{
                git url: 'https://github.com/CHOUPAGUY/project3.git', branch: 'main'
            }
        }
        stage('Build'){
            steps{
                sh 'sudo docker build . -t choupaguy/nodo-todo-app-test:latest'
            }
        }
        stage('Test image') {
            steps {
                echo 'testing...'
                sh 'sudo docker inspect --type=image choupaguy/nodo-todo-app-test:latest '
            }
        }
        
        stage('Push'){
            steps{
        	     sh "sudo docker login -u choupaguy -p dckr_pat_Yc4E-p7Yw0HTkCR1IDSvHB4IIXQ"
                 sh 'sudo docker push choupaguy/nodo-todo-app-test:latest'
            }
        }  
        stage('Deploy'){
            steps{
                echo 'deploying on another server'
                sh 'sudo docker stop nodetodoapp || true'
                sh 'sudo docker rm nodetodoapp || true'
                sh 'sudo docker run -d --name nodetodoapp -p 80:80 choupaguy/nodo-todo-app-test:latest'
                script {
                    sh '''
                    ssh -i /home/ec2-user/workspace/node-todo-app-deployment/instance_key_pair.pem -o StrictHostKeyChecking=no ec2-user@34.201.166.180 <<EOF
                    sudo docker login -u choupaguy -p dckr_pat_Yc4E-p7Yw0HTkCR1IDSvHB4IIXQ
                    sudo docker pull choupaguy/nodo-todo-app-test:latest
                    sudo docker stop nodetodoapp || true
                    sudo docker rm nodetodoapp || true 
                    sudo docker run -d --name nodetodoapp -p 8000:8000 choupaguy/nodo-todo-app-test:latest
                    EOF
                    '''
                }
            }
        }
    }
}