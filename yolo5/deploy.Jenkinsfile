pipeline {
    agent any
    parameters {
        string(name: 'tag', defaultValue: '20', description: 'Docker image tag')
    }
    stages {
        stage('kubeconfig ') {
            steps {
                sh '''
                    aws eks --region us-east-1 update-kubeconfig --name k8s-main
                    kubectl config set-context --current --namespace=joumanakh-k8s
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    cd k8s
                    echo "first step"
                    sed -i "s#image: .*#image: joumanakh/joumanakh-yolo5:${tag}#" yolo5.yaml
                    echo "After modification:"
                    kubectl apply -f yolo5.yaml
                    kubectl apply -f autoscaleHPA.yaml
                '''
            }
        }
    }
}

