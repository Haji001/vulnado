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

    stage('Maven Build'){
      steps{
        sh 'mvn clean install'
      }
    }

    stage('Build the Image') {
      steps {
        script {
          app = docker.build('vulimage')
        }
      }
    }
    stage('Scanning Container Image '){
      steps{
        snykSecurity(
          snykInstallation: 'snyk@latest',
          snykTokenId: 'SNYK_TOKEN',
          failOnIssues: false,
          monitorProjectOnBuild: true,
          additionalArguments: '--container vulimage'

        )
      }
    }
    stage('SCA Analysis....'){
      steps{
        script{
          snykSecurity(
            snykInstallation: 'snyk@latest',
            snykTokenId: 'SNYK_TOKEN',
            failOnIssues: false,
            targetFile: 'pom.xml',
            //additionalArguments: '--all-projects'
          )
        }
      }
    }
    stage('IaC Scanning....'){
      steps{
        script{
          snykSecurity(
            snykInstallation: 'snyk@latest',
            snykTokenId: 'SNYK_TOKEN',
            failOnIssues: false,
            targetFile: 'main.tf',
            additionalArguments: '--command=iac test'
          )
        }
      }
    }
  }
}
