
pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    CI = true
    ARTIFACTORY_ACCESS_TOKEN = credentials('artifactory-access-token')
    MAVEN_HOME = '/opt/apache-maven-3.8.6/bin'
  }
  stages {
    stage('Build') {
      steps {
        script {
          echo "${env.MAVEN_HOME}"
        sh '''
          "${env.MAVEN_HOME}/mvn clean install -X"
          '''
        } //script
      } // step
    } // stage
    stage('Upload to Artifactory') {
      agent {
        docker {
          image 'releases-docker.jfrog.io/jfrog/jfrog-cli-v2:2.2.0' 
          reuseNode true
        }
      }
      steps {
        sh 'jfrog rt upload --url https://imejri.jfrog.io/artifactory/ --access-token ${ARTIFACTORY_ACCESS_TOKEN} target/*.jar java-web-app/'
      }
    }
  }
}
