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
                    sh 'docker build -t nk2603/term .'
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'ay_pass', usernameVariable: 'ay_user')]) {
                        sh 'docker login -u $ay_user -p $ay_pass'
                        sh 'docker push nk2603/term'
                    }
                }
            }
        }

        stage('Deploy Services') {
            steps {
                script {
                    sh 'docker rm -f term'
                    sh 'docker run -d --name term1 -p 2005:80 nk2603/term'
                }
            }
        }
        
        stage('Post Deployment Testing') {
            steps {
                script {
                    sh 'curl -I http://localhost:2005'
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
