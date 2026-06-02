pipeline {
    agent any

    stages {
        stage('Informations') {
            steps {
                echo '===== Informations ====='
                sh 'pwd && ls -la /workspace/proshop-v2'
            }
        }

        stage('Docker Test') {
            steps {
                echo '===== Verification Docker ====='
                sh 'docker --version && docker ps'
            }
        }

        stage('Build Backend') {
            steps {
                echo '===== Build Backend ====='
                sh 'cd /workspace/proshop-v2 && docker build -t proshop-backend:latest -f backend/Dockerfile .'
            }
        }

        stage('Build Frontend') {
            steps {
                echo '===== Build Frontend ====='
                sh 'cd /workspace/proshop-v2 && docker build -t proshop-frontend:latest frontend/'
            }
        }

        stage('Deploy') {
            steps {
                echo '===== Deploiement ====='
                sh 'cd /workspace/proshop-v2 && docker compose down --remove-orphans || true'
                sh 'docker rm -f proshop-prometheus proshop-grafana || true'
                sh 'cd /workspace/proshop-v2 && docker compose up -d'
            }
        }

        stage('Verify') {
            steps {
                echo '===== Verification ====='
                sh 'docker ps'
                sh 'cd /workspace/proshop-v2 && docker compose ps'
            }
        }
    }

    post {
        success {
            echo '================================='
            echo 'PIPELINE EXECUTE AVEC SUCCES'
            echo '================================='
            echo 'Frontend : http://localhost:3000'
            echo 'Backend  : http://localhost:5000'
            echo 'MongoDB  : localhost:27017'
        }
        failure {
            echo '================================='
            echo 'ECHEC DU PIPELINE'
            echo '================================='
        }
    }
}