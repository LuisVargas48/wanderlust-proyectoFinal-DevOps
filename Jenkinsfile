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
                        echo "Downloading kubectl..."
                        curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.31.0/bin/linux/amd64/kubectl
                        chmod +x kubectl
                        mv kubectl $WORKSPACE/kubectl
                        echo "Adding kubectl to PATH..."
                        export PATH=$WORKSPACE:$PATH
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
                    sh '''
                        echo "Setting PATH for kubectl..."
                        export PATH=$WORKSPACE:$PATH
                        echo "Deploying to Kubernetes..."
                        kubectl apply -f Kubernetes/frontend/frontend-deployment.yml
                        kubectl apply -f Kubernetes/frontend/frontend-service.yml
                        kubectl apply -f Kubernetes/mongo/mongo-deployment.yml
                        kubectl apply -f Kubernetes/mongo/mongo-service.yml
                        kubectl apply -f Kubernetes/mongo/configmap/mongo-data.yml
                        kubectl apply -f Kubernetes/redis/redis-deployment.yml
                        kubectl apply -f Kubernetes/redis/redis-service.yml
                        kubectl apply -f Kubernetes/backend/backend-deployment.yml
                        kubectl apply -f Kubernetes/backend/backend-service.yml
                    '''
                }
            }
        }
    }
}
