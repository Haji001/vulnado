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

        stage('checking docker...'){
            steps {
              script {
                sh 'docker --version'
              }
            }
        }
    }
}

//         stage('Test') {
//             steps {
//                 echo 'Testing...'
//                 snykSecurity(
//                     snykInstallation: 'snyk@latest',
//                     snykTokenId: 'snyk-api-token'
//                     // place other parameters here
//                 )
//             }
//         }
//     }
// }

        // Testing stage (commented out)
        /*
        stage('Testing') {
            steps {
                withSonarQubeEnv(installationName: 'SonarQube Host', credentialsId: 'SONAR_TOKEN') {
                    sh 'mvn clean verify sonar:sonar \
                        -Dsonar.projectKey=vulnado \
                        -Dsonar.projectName="vulnado" \
                        -Dsonar.login=$SONAR_TOKEN \
                        -Dsonar.host.url=http://localhost:9000'
                }
            }
        }
        */

        // Build Image stage (commented out)
        /*
        stage('Build Image') {
            steps {
                withDockerRegistry([credentialsId: "dockerlogin", url: "https://hub.docker.com/repository/docker/ant0021/testv1/general"]) {
                    script {
                        sh 'docker build -t ant0021/testv1:tag321'
                    }
                }
            }
        }
        */
  //  }
//}
