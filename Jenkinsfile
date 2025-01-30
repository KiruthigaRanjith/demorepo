pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'kiruthiga1411/demo-0.0.1-snapshot'
        REGISTRY = 'kiruthiga1411'
        K8S_CLUSTER = 'minikube'
        K8S_NAMESPACE = 'default'  // Specify your Kubernetes namespace
        KUBECONFIG_PATH = 'C:/Users/c_kiruthiga/.kube/config'  // Path to kubeconfig file
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/KiruthigaRanjith/demorepo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $REGISTRY/$DOCKER_IMAGE:$GIT_COMMIT .'
                }
            }
        }

        stage('Push to Docker Registry') {
            steps {
                script {
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                    sh 'docker push $REGISTRY/$DOCKER_IMAGE:$GIT_COMMIT'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Deploy the image to Kubernetes
                    sh "kubectl --kubeconfig=$KUBECONFIG_PATH set image deployment/your-deployment your-app=$REGISTRY/$DOCKER_IMAGE:$GIT_COMMIT -n $K8S_NAMESPACE"
                    sh "kubectl --kubeconfig=$KUBECONFIG_PATH rollout restart deployment/your-deployment -n $K8S_NAMESPACE"
                }
            }
        }
    }

    post {
        always {
            cleanWs()  // Clean workspace after the build
        }
    }
}
