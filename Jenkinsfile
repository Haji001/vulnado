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
    // TESTING
    // stage('Run container scan'){
    //   steps{
    //     echo "Testing......"
    //     snykSecurity(
    //       snykInstallation: 'snyk@latest',
    //       snykTokenId: 'SNYK_TOKEN',
    //       failOnIssues: false,
    //       monitorProjectOnBuild: true,
    //       additionalArguments: '--all-projects --d'
        //)
      }
    }
    stage('Run Container Scan') {
      steps{
          sh 'snyk container test vulimage/test'
        }
      }
    