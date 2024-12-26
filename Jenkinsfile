pipeline {
    agent any
   
    
    stages {
         stage('Whoami y LS') {
            steps {
               sh 'whoami'
                sh 'ls'
            }
        }
        stage('Checkout Code') {
            steps {
                // Clona el repositorio de GitHub en la rama main
                git branch: 'main', url: 'https://github.com/LuisVargas48/kubernetes-FUNCIONAL'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Asegúrate de que kubectl esté configurado en tu máquina Jenkins o que tenga acceso al clúster
                    // Aplica los manifiestos YAML para el backend, frontend, MongoDB y Redis
                   
                    sh 'kubectl apply -f Kubernetes/frontend/frontend-deployment.yml'
                    sh 'kubectl apply -f Kubernetes/frontend/frontend-service.yml'
                    
                    sh 'kubectl apply -f Kubernetes/mongo/mongo-deployment.yml'
                    sh 'kubectl apply -f Kubernetes/mongo/mongo-service.yml'
                    sh "kubectl apply -f Kubernetes/mongo/configmap/mongo-data.yml"
                    
                    sh 'kubectl apply -f Kubernetes/redis/redis-deployment.yml'
                    sh 'kubectl apply -f Kubernetes/redis/redis-service.yml'

                    sh 'kubectl apply -f Kubernetes/backend/backend-deployment.yml '
                    sh 'kubectl apply -f Kubernetes/backend/backend-service.yml'
                    
                }
            }
        }
    }
    
}
