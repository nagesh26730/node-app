pipeline{
    agent any
    environment{
        DOCKER_TAG = getDockerTag()
    }
    stages{
        stage('Build docker image'){
            steps{
                sh "docker build . -t nagesh143/nodeapp:${DOCKER_TAG}"
            }
        }
         stage('Docker push images'){
            steps{
                withCredentials([string(credentialsId: 'docker-pwd', variable: 'DockerHubPwd')]) {
                sh "docker login -u nagesh143 -p ${DockerHubPwd}"
                sh "docker push nagesh143/nodeapp:${DOCKER_TAG}"
            }
        }
              stage('Deploy to k8s'){
            steps{
                
                sh "chmod +x changeTag.sh"
                sh "./changeTag.sh ${DOCKER_TAG}"
                sshagent(['k8-eks']) {
                       sh "scp -o StrictHostKeyChecking=no services.yml node-app-pod.yml ec2-user@ 172.31.5.160:/home/ec2-user/"
                    script{
                        try{
                        sh "ssh ec2-user@ 172.31.5.160 kubectl apply -f ."
                        }catch(error){
                         sh "ssh ec2-user@ 172.31.5.160 kubectl create -f ."   
                        }
                    }
                  }
        }
    }
}
}

    def getDockerTag(){
        def tag = sh script: 'git rev-parse HEAD', returnstd: true
        return tag
     }
