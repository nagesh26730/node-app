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
        stage('Docker image push to Docker hub'){
            steps{
            withCredentials([string(credentialsId:'docker-pwd', variable: 'Docker_pwd')]) {
               sh "docker login -u nagesh143 -p ${Docker_pwd}"
               sh "docker push nagesh143/nodeapp:${DOCKER_TAG}"
           }
           }    
    }
        stage('Deploy to k8'){
            steps{
               sh "chmod +x changeTag.sh"
                sh "./changeTag.sh ${DOCKER_TAG}"
                sshagent(['kops-machine']) {
                       sh "ssh -o StrictHostKeyChecking=no services.yml node-app-pod.yml ec2-user@3.110.104.28:/home/ec2-user/"
                    script{
                        try{
                            
                           sh " ssh ec2-user:3.110.104.28:/home/ec2-user kubectl apply -f ."
                        }catch(error){
                            sh " ssh ec2-user:3.110.104.28:/home/ec2-user kubectl create -f ."
                        }
                    }
                 }
            }
        }
            
}
}
def getversionTag(){
 def tag = sh script: 'git rev-parse HEAD', retirnStdout: true
 return tag
}
