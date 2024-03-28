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
    
    stage('Iac Scanning'){
      steps{
        sh 'checkov -s -f main.tf'
      }
    }
    
    // Adding the GitGuardian scan stage
    stage('GitGuardian Scan') {
      steps {
        script {
          withCredentials([string(credentialsId: 'GGSHIELD_TOKEN', variable: 'GITGUARDIAN_API_KEY')]) {
            sh 'ggshield secret scan ci'
          }
        }
      }
    }
  }
}