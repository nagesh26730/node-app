pipeline{
    agent any
    environment{
        DOCKER_TAG = getversionTag()
    }
    stages{
        stage('Docker Build images'){
            steps{
                sh "docker build . -t nagesh143/nodeapp:${DOCKER_TAG}"
            }
        }
        
    }
}

def getversionTag(){
 def tag = sh script: 'git rev-parse HEAD', retirnStdout: true
 return tag
}
