pipeline {
    agent any
    environment{
       IMAGE_NAME = 'joumanakh-yolo5'
       USR = 'joumanakh'
    }

    stages {
        stage('Build') {
            steps {
                   withCredentials([string(credentialsId: 'dtoken', variable: 'DOCKERTOKEN')]) {
                       sh '''
                       docker login -u $USR -p $DOCKERTOKEN
                       docker build -t yolo5:$BUILD_NUMBER yolo5/
                       docker tag yolo5:$BUILD_NUMBER $USR/$IMAGE_NAME:$BUILD_NUMBER
                       docker push $USR/$IMAGE_NAME:$BUILD_NUMBER
                       '''

                }
            }
        }
        stage('Trigger Deploy') {
            steps {
                build job: 'YoloDeploy', wait: false, parameters: [
                    string(name: 'tag', value: "$BUILD_NUMBER")
        ]
    }
}

    }
}

