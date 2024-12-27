pipeline {
    agent any

    stages {
        stage('Whoami y LS') {
            steps {
                sh 'whoami'
                sh 'ls'
            }
        }
        stage('Install kubectl') {
    steps {
        script {
            sh '''
                echo "Checking if kubectl exists..."
                if [ ! -f $WORKSPACE/kubectl ]; then
                    echo "Downloading kubectl..."
                    curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.31.0/bin/linux/amd64/kubectl
                    chmod +x kubectl
                    mv kubectl $WORKSPACE/kubectl
                else
                    echo "kubectl already exists in $WORKSPACE"
                fi
                echo "Adding kubectl to PATH..."
                export PATH=$WORKSPACE:$PATH
                echo "Verifying kubectl installation..."
                kubectl version --client
            '''
        }
    }
}

        stage('Checkout Code') {
            steps {
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
                    sh "kubectl replace -f Kubernetes/mongo/configmap/mongo-data.yml"
                    
                    sh 'kubectl apply -f Kubernetes/redis/redis-deployment.yml'
                    sh 'kubectl apply -f Kubernetes/redis/redis-service.yml'

                    sh 'kubectl apply -f Kubernetes/backend/backend-deployment.yml '
                    sh 'kubectl apply -f Kubernetes/backend/backend-service.yml'
                    
                }
            }
        }
    }
}
