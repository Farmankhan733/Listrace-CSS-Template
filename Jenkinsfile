pipeline {
    agent any

       options {
        disableResume()
        timestamps()
    }
    
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
                    mkdir -p /var/lib/jenkins/html/
                    rm -rf /var/lib/jenkins/html/*
                    cp -r * /var/lib/jenkins/html/
                '''
            }
        }

        stage('Reload NGINX') {
            steps {
                sh '''
                    echo "Reloading NGINX to apply changes..."
                    systemctl restart nginx
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
