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
                    sed -i "s#image: .*#image: joumanakh/joumanakh-polybot:${tag}#" polybot.yaml
                    echo "After modification:"
                    kubectl apply -f polybot.yaml
                    kubectl apply -f ingress.yaml
                '''
            }
        }
    }
}
