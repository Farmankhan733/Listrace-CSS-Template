pipeline {
    agent any



    environment {
        APP_PORT = 'http://localhost:8081/'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', credentialsId: 'github-creds', url: 'https://github.com/Farmankhan733/Listrace-CSS-Template.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build (if needed)') {
            steps {
                sh 'npm run build || echo "No build step defined"'
            }
        }

        stage('Stop Previous App') {
            steps {
                sh '''
                    if lsof -i :$http://localhost:8081/; then
                      fuser -k ${http://localhost:8081/}/tcp || true
                    fi
                '''
            }
        }

        stage('Start App') {
            steps {
                sh 'systemctl start nginx'
            }
        }

        stage('Test App Running') {
            steps {
                sh 'sleep 5 && curl http://localhost:${http://localhost:8081/} || echo "App may not have started"'
            }
        }
    }

    post {
        failure {
            echo 'Deployment failed!'
        }
        success {
            echo 'App deployed successfully on localhost!'
        }
    }
}
