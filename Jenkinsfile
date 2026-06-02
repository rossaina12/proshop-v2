pipeline { 
    agent any 
    stages { 
       stage('Deploy') {
            steps {
                echo '===== Deploiement ====='
                sh '''
                    cd $PROJECT_DIR
                    docker compose down --remove-orphans || true
                    docker stop proshop-prometheus proshop-grafana || true
                    docker rm proshop-prometheus proshop-grafana || true
                    docker compose up -d
                '''
           }
        }
     }       
}
