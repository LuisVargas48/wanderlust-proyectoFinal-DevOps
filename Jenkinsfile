pipeline {
    agent {
         kubernetes {
            namespace 'jenkins'
            defaultContainer 'kubectl'
            yaml """
            apiVersion: v1
            kind: Pod
            spec:
                containers:
                - name: kubectl
                  image: bitnami/kubectl:1.32-debian-12
                  command:
                   - "/bin/sh"
                  tty: true
                  securityContext:
                    runAsUser: 0
                  volumeMounts:
                    - name: config
                      mountPath: /.kube/
                restartPolicy: Never
                volumes:
                - name: config
                  secret:
                    secretName: kubeconfig-secret
                    items:
                    - key: kubeconfig
                      path: config
            """
        }
    }

    stages{

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/LuisVargas48/kubernetes-FUNCIONAL'
            }
        }

        stage('Setting ConfigMap to MongoDB') {
            steps {
                script {
                    IS_CONFIGMAP_MONGO_DATA_EXISTS=sh(
                        script: 'kubectl get configmap -n default -o jsonpath="{range .items[?(@.metadata.name == \\"mongo-data\\")]}{.metadata.name}{end}"',
                        returnStdout: true
                    ).trim()
                    
                    if (IS_CONFIGMAP_MONGO_DATA_EXISTS == 'mongo-data') {
                        echo 'ConfigMap mongo-data already exists'
                    } else {
                        echo 'Creating ConfigMap mongo-data'
                        sh "kubectl apply -f Kubernetes/mongo/configmap/mongo-data.yml"
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Verifica si kubectl está disponible en el PATH
                    sh 'which kubectl'  // Esto debería devolver la ruta de kubectl
                    sh 'kubectl version --client'
                    
                    // Aplicar los manifiestos YAML para el despliegue
                    sh 'kubectl apply -f Kubernetes/frontend/frontend-deployment.yml -n default'
                    sh 'kubectl apply -f Kubernetes/frontend/frontend-service.yml -n default'
                    
                    sh 'kubectl apply -f Kubernetes/mongo/mongo-deployment.yml -n default'
                    sh 'kubectl apply -f Kubernetes/mongo/mongo-service.yml -n default'
                    
                    sh 'kubectl apply -f Kubernetes/redis/redis-deployment.yml -n default'
                    sh 'kubectl apply -f Kubernetes/redis/redis-service.yml -n default'

                    sh 'kubectl apply -f Kubernetes/backend/backend-deployment.yml -n default'
                    sh 'kubectl apply -f Kubernetes/backend/backend-service.yml -n default'
                }
            }
        }
    }

}
