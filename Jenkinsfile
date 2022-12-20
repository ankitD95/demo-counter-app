pipeline{
    agent any
    stages{

        stage('git checkout')
        {
            steps{
                git branch: 'main', url: 'https://github.com/ankitD95/demo-counter-app.git'
            }
        }

        stage('UNIT Test')
        {
            steps{
                bat 'mvn test'
            }
        }
        stage('Integration Test')
        {
            steps{
                bat 'mvn verify -DskipUnitTests'
            }
        }
        stage('maven Build')
        {
            steps{
                bat 'mvn clean install'
            }
        }
        stage('static code analysis')
        {
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-api') {
                    bat 'mvn clean package sonar:sonar'
                                }
                            }
                }

        }
        stage('Quality Gate Check')
        {
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
                }
            }
        }
        stage('upload jar file to nexus')
        {
            steps{
                script{

                    def readPomVersion = readMavenPom file: 'pom.xml'


                    def nexurepo = readMavenPom.version.endswith("snapshot") ? "demoapp-snapshot" : "demoapp-release"
                   nexusArtifactUploader artifacts:
                   [[artifactId: 'sprringboot',
                    classifier: '',
                     file: 'target/Uber.jar',
                      type: 'jar']],
                       credentialsId: 'nexus-auth',
                        groupId: 'com.example',
                         nexusUrl: '54.209.86.119:8081',
                          nexusVersion: 'nexus3',
                           protocol: 'http',
                            repository: 'demoapp-release',
                             version: "${readPomVersion.version}"
                }
            }
        }

    }
}