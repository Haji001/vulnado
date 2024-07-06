

pipeline {
  environment { 
    imagename = 'dirtyimage/jenkins'
  }
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

    stage('GitGuardian Scan') {
      steps {
        script {
          withCredentials([string(credentialsId: 'GGSHIELD_TOKEN', variable: 'GITGUARDIAN_API_KEY')]) {
            
            def result = sh(script: 'ggshield secret scan path . --recursive --yes', returnStatus: true)
            if (result !=0){
              echo "Secrets detected or error occurred, please review. Exit code: ${result}"
            }

            //sh 'ggshield secret scan path . --recursive --yes'
          }
        }
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
          dockerImage = docker.build "${imagename}:latest"
        }
      }
    }
    
    stage('Scanning Container Image...'){
      steps{
        snykSecurity(
          snykInstallation: 'snyk@latest',
          snykTokenId: 'SNYK_TOKEN',
          failOnIssues: false,
          monitorProjectOnBuild: true,
          additionalArguments: '--container dirtyimage'
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
          )
        }
      }
    }
    
    stage('Iac Scanning'){
      steps{
        sh 'checkov -s -f main.tf'
      }
    }
  }
}
    
