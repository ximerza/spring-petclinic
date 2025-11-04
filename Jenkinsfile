pipeline {
  agent none

  stages {

    stage('Maven Install') {
      agent {
        docker {
          image 'maven:3.9-eclipse-temurin-25'
          reuseNode true
        }
      }
      steps {
        sh 'mvn clean install -DskipTests'
      }
    }

    stage('Docker Build') {
      agent any
      steps {
        sh 'docker build -t ximerza/spring-petclinic:latest .'
      }
    }

    stage('Docker Push') {
      agent any
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "docker login -u ${dockerHubUser} -p ${dockerHubPassword}"
          sh 'docker push ximerza/spring-petclinic:latest'
        }
      }
    }

  }
}
