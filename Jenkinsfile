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

    stage('snyk version...'){
      steps {
        script {
          sh 'snyk --version'
        }
      }
    }
  }
}
// testing

    // stage('Testing') {
    //   steps {
    //     withSonarQubeEnv(installationName: 'SonarQube Host', credentialsId: 'SONAR_TOKEN') {
    //       sh 'mvn clean verify sonar:sonar \
    //            -Dsonar.projectKey=vulnado \
    //            -Dsonar.projectName="vulnado" \
    //            -Dsonar.login=$SONAR_TOKEN \
    //            -Dsonar.host.url=http://localhost:9000'
    //     }
    //   }
    // }

//     stage('Build Image') { // Notice the capital "I" here
//       steps {
//         withDockerRegistry([credentialsId: "dockerlogin", url: "https://hub.docker.com/repository/docker/ant0021/testv1/general"]) {
//           script {
//             sh 'docker build -t ant0021/testv1:tag321'
//           }
//         }
//       }
//     }
//   }
// }
