pipeline {
    agent any 
    environment { 
      name = 'docker.myartifactory.com'
       //name= 'mill.jfrog.info:12157'
    }
    stages {
        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "Arti1",
                    url: "http://myartifactory.com/artifactory",
                    credentialsId: "CREDENTIALS"
                )
            }
        }
        stage('Building Images') {
            steps {
                sh """
                docker build -t ${name}/ansible-2.8.5:latest --build-arg ansible=2.8.5 . --no-cache
                """
            }
        }
        stage('Pushing and publishing into Jfrog'){
            steps
            {
                rtDockerPush(
                  serverId: 'Arti1',
                  image: "${name}/ansible-2.8.5:latest",
                  targetRepo: 'docker-local/')
                
                /*
                script{
                    def server = Artifactory.server "Arti1"
                def rtDocker = Artifactory.docker server: server
                def buildInfo =  rtDocker.push("${name}/ansible-2.8.5:latest", "docker-local")

            }
           rtDockerPush(
                  serverId: 'Arti1',
                  image: '${name}/ansible-2.8.5:latest',
                  targetRepo: 'docker-local'
                     )       */
        }
        }
        
        stage ('Publish build info') {
         steps {
            rtPublishBuildInfo (
                serverId: "Arti1"
            )
          }
         }
        
    }
}
