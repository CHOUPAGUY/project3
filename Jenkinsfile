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
                sh 'sudo docker run -d --name nodetodoapp choupaguy/nodo-todo-app-test:latest'
                sh '''
                ssh -i /c/Users/guyla/pro-ans/devops/instance_key_pair.pem -o StrictHostKeyChecking=no ec2-user@ec34.201.166.180 <<EOF
                sudo docker login -u choupaguy -p dckr_pat_Yc4E-p7Yw0HTkCR1IDSvHB4IIXQ
                sudo docker pull choupaguy/nodo-todo-app-test:latest
                sudo docker stop nodetodoapp || true
                sudo docker rm nodetodoapp || true 
                sudo docker run -d --name nodetodoapp choupaguy/nodo-todo-app-test:latest
                EOF
                '''
            }
        }
    }
}
