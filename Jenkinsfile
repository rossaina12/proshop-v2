stage('Deploy') { 
    steps { 
       sh ''' 
       cd /workspace/proshop-v2 
       docker compose down
       docker compose up -d --build 
           ''' 
     } 
} 