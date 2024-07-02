pipeline {
    agent any

    stages {
        stage('Git_clone') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/narharigtm/Devops-react-java-mysql.git']])
            }
        }
        stage('Build and clean') {
            steps {
                script {
                    def mvnHome = tool name: 'maven', type: 'maven'
                    sh ''' cd lakeSide-hotel-demo-server-master ${mvnHome}/bin/mvn clean package   ${mvnHome}/bin/mvn clean '''
               }
            }
        }
        stage('backend-sonarqube-analysis') {
            steps {
                script {
                    def scannerHome = tool name: 'sonarqube', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withEnv(["PATH+SONAR=${scannerHome}/bin"]) {
                        sh '''sonar-scanner -Dsonar.host.url=http://localhost:9001 -Dsonar.login=sqp_5407eabb380bd3bd842a2025e657a652c61d4aa8 -Dsonar.projectKey=javaBackend -Dsonar.projectName=javaBackend -Dsonar.sources=./lakeSide-hotel-demo-server-master -Dsonar.java.binaries=.'''
                    }
                }
            }
        }
        stage('frontend-sonarqube-analysis') {
            steps {
                script {
                    def scannerHome = tool name: 'sonarqube', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withEnv(["PATH+SONAR=${scannerHome}/bin"]) {
                        sh '''sonar-scanner -Dsonar.host.url=http://localhost:9001 -Dsonar.login=sqp_683d404cc57dce6b04ce7c6db4c0a86bbe9bf782 -Dsonar.projectKey=reactFrontend -Dsonar.projectName=reactFrontend -Dsonar.sources=./lakeSide-hotel-demo-client '''
                    }
                }
            }
        }
    }   
}
