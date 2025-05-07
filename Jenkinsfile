pipeline {
    agent any

     options {
        disableResume()
    }

     tools {
        nodejs 'Nodejs18'
    }
    
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
                sh '''
                    echo "Installing dependencies..."
                    npm install || { echo "Install failed"; exit 1; }
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                    echo "Building the project..."
                    GENERATE_SOURCEMAP=false npm run build || { echo "Build failed"; exit 1; }
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    echo "Deploying to /var/lib/jenkins/..."
                    sudo mkdir -p /var/lib/jenkins/
                    sudo rm -rf /var/lib/jenkins/*
                    sudo cp -r build/* /var/lib/jenkins/
                '''
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

        stage('Reload NGINX') {
            steps {
                sh '''
                    echo "Reloading NGINX to serve updated React app..."
                    sudo systemctl reload nginx
                '''
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
