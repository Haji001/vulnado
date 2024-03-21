pipeline {
  agent any

  tools {
    maven 'MAVEN_3.9.6'
  }

  stages {
    stage('Clean WS') {
      steps {
        script {
          cleanWs()
        }
      }
    }

    stage('Checkout') {
      steps {
        // Checkout the source code from the GitHub repository
        checkout([
          $class: 'GitSCM',
          branches: [[name: '*/test001']],
          userRemoteConfigs: [[url: 'https://github.com/Haji001/vulnado.git']]
        ])
      }
    }
    stage('check for versions') {
      steps {
        script {
          app = docker.build('vulimage/test')
        }
      }
    }
    stage('Run container scan') {
      steps{
        withCredentials([string(credentialsID: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
          script {
            try {
              sh "snyk containter test vulimage/test"
            }  catch (err) {
              echo err.getMessage()
            }
          }
        }
      }
    }
  }
}
