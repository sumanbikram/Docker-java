pipeline {
  environment {
    imagename = "GCR/java"
    registryCredential = 'xxxx-NexusREGISTRY'
    dockerImage = ''
  }
  agent any
  tools {
        maven "Maven 3.8.5"
        jdk "JDK 9.0.4"
    }
    stages {
        stage('Initialize'){
            steps{
                echo "PATH = ${M2_HOME}/bin:${PATH}"
                echo "M2_HOME = /opt/maven"
            }
        }
        stage('Build') {
            steps {
                dir("/var/lib/jenkins/workspace/demopipelinetask/my-app") {
                sh 'mvn -B -DskipTests clean package'
                }
            }
        }
     }
  stages {
    stage('Cloning Git') {
      steps {
        git credentialsId: 'bef7a92a-8e0e-42f4-9159-04d4a9beb28c', url: 'https://github.com/sumanbikram/Docker-java.git'


      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build imagename
        }
      }
    }
    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')

          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $imagename:$BUILD_NUMBER"
         sh "docker rmi $imagename:latest"

      }
    }
  }
}
