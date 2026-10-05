pipeline {

    agent any

    stages {

        stage('Check Tools') {
            steps {
                sh 'docker --version'
                sh 'kubectl version --client'
                sh 'minikube version'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t jenkins-demo:latest .'
            }
        }

        stage('Load Image into Minikube') {
            steps {
                sh '''
                    docker save jenkins-demo:latest | \
                    docker exec -i minikube ctr -n k8s.io images import -
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'kubectl get deployment jenkins-demo'
                sh 'kubectl get pods -l app=jenkins-demo'
                sh 'kubectl get service jenkins-demo-service'
            }
        }
    }
}