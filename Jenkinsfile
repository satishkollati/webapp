pipeline{
    agent any
    tools {
      maven 'maven3'
         }
         environment{
                  DOCKER_TAG = getVersion()
         }
    stages{
        stage("git fertch"){
            steps{
            git "https://github.com/satishkollati/webapp.git"
            }
        }
        stage("build the code"){
            steps{
                sh "mvn clean install"
            }
        }
        stage("docker build"){
            steps{
                sh "docker build . -t satishkollati/tomcat:${DOCKER_TAG}"
            }
        }
        stage("push an image"){
            steps{
                withCredentials([string(credentialsId: 'docker-cred', variable: 'dockerHubPwd')]) {
                sh "docker login -u satishkollati -p ${dockerHubPwd}"
                  } 
                  sh "docker push satishkollati/tomcat:${DOCKER_TAG} " 
            }
        }
        
    }
}

    def getVersion(){
    def commitHash = sh label: '', returnStdout: true, script: 'git rev-parse --short HEAD'
    return commitHash
}
