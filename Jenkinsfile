pipeline {
    agent any 
    environment { 
      name = 'docker.myartifactory.com'
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
        stage('Pushing images to Jfrog'){
            steps
            {
                script{
                    def server = Artifactory.server "Arti1"
                def rtDocker = Artifactory.docker server: server
                def buildInfo =  rtDocker.push("${name}/ansible-2.8.5:latest", "docker-local")
                server.publishBuildInfo buildInfo

            }
        }
     //   stage ('Publish build info') {
      //    steps {
       //     rtPublishBuildInfo (
        //        serverId: "Arti1"
         //   )
         // }
       // }
    }
}
