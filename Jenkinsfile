pipeline{

    agent any
    environment {
        PATH = "/opt/maven3.6/bin/:$PATH"
    }

    stages {

        stage('Git Checkout'){

            steps{

                script{

                    git branch: 'master', url: 'https://github.com/buddipammu2616/pammubuddi.git'
                }
            }
        }
        stage('UNIT testing'){

            steps{

                script{

                    sh 'mvn test'
                }
            }
        }
        stage('Integration testing'){

            steps{

                script{

                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
        stage('build code') {
            steps {
                sh 'mvn clean install'
}
}

pipeline{

    agent any
    environment {
        PATH = "/opt/maven3.6/bin/:$PATH"
    }

    stages {

        stage('Git Checkout'){

            steps{

                script{

                    git branch: 'master', url: 'https://github.com/buddipammu2616/pammubuddi.git'
                }
            }
        }
        stage('UNIT testing'){

            steps{

                script{

                    sh 'mvn test'
                }
            }
        }
        stage('Integration testing'){

            steps{

                script{

                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
        stage('build code') {
            steps {
                sh 'mvn clean install'
          }
      }
 stage('soanr qube analysis') {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-key') {
                      sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }
        stage('qaulity gate check') {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-key'
                }
            }
        }
        stage ('war file upload') {
            steps {
                script {
                    def readpomversion = readMavenPom file: 'pom.xml'
                    def nexusrepo = readpomversion.version.endsWith("SNAPSHOT") ? "app-release" : "app-snapshot"
                    nexusArtifactUploader artifacts:
                        [
                            [
                                artifactId: 'buddipammu',
                                classifier: '', file:'target/buddipammu.war',
                                type: 'war'
                            ]
                        ],
                        credentialsId: 'nexus-credentials',
                        groupId: 'com.ustglobal',
                        nexusUrl: '65.2.183.206:8081',
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        repository: 'nexusrepo',
                        version: "${readpomversion.version}"
                }
            }
        }

      }
    }
