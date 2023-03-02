pipeline {
    agent any
    stages {
        stage('build da imagem docker'){
            steps{
                sh 'docker build -t devops/app .'
            }
        }
        stage('subir docker compose - redis e app'){
            steps {
                sh 'docker-compose up --build -d'
            }
        }
        stage('sleep para subida de containers'){
            steps{
                sh 'sleep 10'
            }
        }
        stage('Sonarqube validation'){
            steps{
                script{
                    scannerHome = tool 'sonar-scanner';
                }
                withSonarQubeEnv('sonar-server'){
                    sh "$(scannerHome)/bin/sonar-scanner -Dsonar.projectkey=redis-app -Dsonar.sources=. -Dsonar.host.url= "http://192.168.1.6" -Dsonar.login="admin""
            }
        } 
        stage('teste da aplicação'){
            steps{
                sh 'chmod +x teste-app.sh'
                sh './teste-app.sh'
            }
        }
       stage('showtdown dos containers'){
           steps{
               sh 'docker-compose down'
           }
       }        
    }   
}
