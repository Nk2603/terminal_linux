pipeline{
 agent any
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', poll: false, url: 'https://github.com/Nk2603/terminal_linux.git'
            }
        }

        stage('Build and Push Images') {
            steps {
                script {
                    sh 'docker build -t Nk2603/terminal .'
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'ay_pass', usernameVariable: 'ay_user')]) {
                        sh 'docker login -u $ay_user -p $ay_pass'
                        sh 'docker push Nk2603/terminal '
                    }
                }
            }
        }

        stage('Deploy Services') {
            steps {
                script {
                    sh 'docker rm -f terminal_linux '
                    sh 'docker run -d --name terminal_linux -p 2026:80 Nk2603/terminal'
                }
            }
        }
        
        stage('Post Deployment Testing') {
            steps {
                script {
                    sh 'curl -I http://localhost:2026'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}
