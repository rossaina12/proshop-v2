pipeline {
    agent any

    stages {
        stage('Informations') {
            steps {
                echo '===== Informations ====='
                sh '''
                    pwd
                    ls -la
                '''
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
                sh '''
                    cd $WORKSPACE
                    docker build -t proshop-backend:latest -f backend/Dockerfile .
                '''
            }
        }

        stage('Build Frontend') {
            steps {
                echo '===== Build Frontend ====='
                sh '''
                    cd $WORKSPACE
                    docker build -t proshop-frontend:latest frontend/
                '''
            }
        }

stage('Deploy') {
    steps {
        echo '===== Deploiement ====='
        sh '''
            cd $WORKSPACE
            docker compose down --remove-orphans || true
            docker rm -f proshop-mongo proshop-backend proshop-frontend proshop-prometheus proshop-grafana 2>/dev/null || true
            docker volume rm proshop-ci-cd_prometheus-data 2>/dev/null || true
            docker volume create proshop-ci-cd_prometheus-data
            docker run --rm -v proshop-ci-cd_prometheus-data:/etc/prometheus busybox sh -c "cat > /etc/prometheus/prometheus.yml << 'EOF'
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: prometheus
    static_configs:
      - targets: [localhost:9090]

  - job_name: docker
    static_configs:
      - targets: [host.docker.internal:9323]
EOF"
            docker compose up -d
        '''
    }
}
        stage('Verify') {
            steps {
                echo '===== Verification ====='
                sh '''
                    sleep 10
                    docker ps
                    cd $WORKSPACE
                    docker compose ps
                '''
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