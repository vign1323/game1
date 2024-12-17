pipeline {
    agent { label 'k8s-server' }
    stages {
        stage('Pull Code From GitHub') {
            steps {
                git 'https://github.com/vign1323/game1.git'
            }
        }
        stage('Build the Docker image') {
            steps {
                sh 'sudo docker build -t mazeimage /root/project/workspace/game-proj'
                sh 'sudo docker tag mazeimage vign1323/mazeimage:latest'
                sh 'sudo docker tag mazeimage vign1323/mazeimage:${BUILD_NUMBER}'
            }
        }
        stage('Trivy Vulnerability Scan') {
            steps {
                script {
                    sh 'sudo trivy image vign1323/mazeimage:latest'
                    sh 'sudo trivy image vign1323/mazeimage:${BUILD_NUMBER}'
                }
            }
        }
        stage('Push the Docker image') {
            steps {
                sh 'sudo docker image push vign1323/mazeimage:latest'
                sh 'sudo docker image push vign1323/mazeimage:${BUILD_NUMBER}'
            }
        }
        stage('Deploy on Kubernetes') {
            steps {
                sh 'sudo kubectl apply -f /root/project/workspace/k8s-game/pod.yml'
                sh 'sudo kubectl rollout restart deployment loadbalancer-pod'
            }
        }
    }
}
