pipeline{
    agent any
    environment{
        DOCKER_TAG = getDockerTag()
    }
    stages{
        stage('Build docker image'){
            steps{
                sh "docker build -t . nagesh143/nodeapp:${DOCKER_TAG}"
            }
        }
    }
}

    def getDockerTag(){
        def tag = sh script: 'git rev-parse HEAD', returnstd: true
        return tag
     }
