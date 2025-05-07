pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', credentialsId: 'github-creds', url: 'https://github.com/Farmankhan733/Listrace-CSS-Template.git'
            }
        }

        stage('Deploy Static Files') {
            steps {
                sh '''
                    echo "Deploying static files to /var/lib/jenkins/html/..."
                    sudo mkdir -p /var/lib/jenkins/html/
                    sudo rm -rf /var/lib/jenkins/html/*
                    sudo cp -r * /var/lib/jenkins/html/
                '''
            }
        }

        stage('Reload NGINX') {
            steps {
                sh '''
                    echo "Reloading NGINX to apply changes..."
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
            echo 'Static website deployed successfully!'
        }
    }
}
