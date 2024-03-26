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
    stage('checking for container scans...'){
      steps{
        script{
          sh "snyk container test vulimage/test"
        }
      }
    }
  }
  //   stage('scan'){
  //     steps{
  //       script{
  //         snykSecurity severity: 'critical', snykInstallation: 'snyk@latest', snykTokenId: 'SNYK_TOKEN'
  //         def variable = sh(
  //           script: 'snyk container test vulimage/test --severity-threshold=critical',
  //           returnStatus: true
  //         )
  //           echo "error code = ${variable}"
  //           if (variable != 0) {
  //             echo "Alert for vulnerability found"
  //           }
  //       }
  //     }
  //   }
  // }
}

    