pipeline {
    agent any
    environment{
       IMAGE_NAME = 'joumanakh-polybot'
       USR = 'joumanakh'
    }

    stages {
        stage('Build') {
            steps {
                    withCredentials([string(credentialsId: 'dtoken', variable: 'DOCKERTOKEN')]) {
                       sh '''
                       docker login -u $USR -p $DOCKERTOKEN           
                       docker build -t polybot:$BUILD_NUMBER polybot/
                       docker tag polybot:$BUILD_NUMBER $USR/$IMAGE_NAME:$BUILD_NUMBER
                       docker push $USR/$IMAGE_NAME:$BUILD_NUMBER
                       '''


            }
        }
        stage('Trigger Deploy') {
            steps {
                build job: 'PolybotDeploy', wait: false, parameters: [
                    string(name: 'tag', value: "$BUILD_NUMBER")
        ]
    }
}

    }
}
