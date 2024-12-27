pipeline {
    agent any

    environment {
        // Definir la ruta de kubectl en el entorno global
        KUBECTL_PATH = "${WORKSPACE}/kubectl"
        PATH = "${WORKSPACE}:${env.PATH}"
    }

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
                        if [ ! -f $KUBECTL_PATH ]; then
                            echo "Downloading kubectl..."
                            curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.31.0/bin/linux/amd64/kubectl
                            chmod +x kubectl
                            mv kubectl $KUBECTL_PATH
                        else
                            echo "kubectl already exists in $KUBECTL_PATH"
                        fi
                        echo "Adding kubectl to PATH..."
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
                    // Verifica si kubectl está disponible en el PATH
                    sh 'which kubectl'  // Esto debería devolver la ruta de kubectl
                    sh 'kubectl version --client'
                    
                    // Aplicar los manifiestos YAML para el despliegue
                    sh 'kubectl apply -f Kubernetes/frontend/frontend-deployment.yml'
                    sh 'kubectl apply -f Kubernetes/frontend/frontend-service.yml'
                    
                    sh 'kubectl apply -f Kubernetes/mongo/mongo-deployment.yml'
                    sh 'kubectl apply -f Kubernetes/mongo/mongo-service.yml'
                    sh "kubectl apply -f Kubernetes/mongo/configmap/mongo-data.yml"
                    
                    sh 'kubectl apply -f Kubernetes/redis/redis-deployment.yml'
                    sh 'kubectl apply -f Kubernetes/redis/redis-service.yml'

                    sh 'kubectl apply -f Kubernetes/backend/backend-deployment.yml'
                    sh 'kubectl apply -f Kubernetes/backend/backend-service.yml'
                }
            }
        }
    }
}
