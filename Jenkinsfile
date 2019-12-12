pipeline {
    agent { node { label 'deploy-cm' } }
    environment { 
      name = 'docker.myartifactory.com/artifactory/jenkins/slaves/ansible'
    }
    stages {
        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "Arti1",
                    url: "http://myartifactory.com/artifactory",
                    credentialsId: "dockerreg"
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
                rtDockerPush(
                  serverId: 'Arti1',
                  image: "${name}/ansible-2.8.5:latest",
                  targetRepo: 'docker-local/'
                )
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
