pipeline {
    agent any

    tools {
        nodejs 'nodejs'  // Must match the tool name in Jenkins
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
                    echo "Deploying to /var/lib/jenkins/deploy/..."
                    sudo mkdir -p /var/lib/jenkins/deploy/
                    sudo rm -rf /var/lib/jenkins/deploy/*
                    sudo cp -r build/* /var/lib/jenkins/deploy/
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
