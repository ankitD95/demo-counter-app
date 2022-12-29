pipeline{
    agent any
    stages{

        stage('git checkout')
        {
            steps{
                git branch: 'FeatureA', url: 'https://github.com/ankitD95/demo-counter-app.git'
            }
        }

        stage('UNIT Test')
        {
            steps{
                bat 'mvn test'
            }
        }
        stage('INTEGRATION Test')
        {
            steps{
                bat 'mvn verify -DskipUnitTests'
            }
        }
        stage('Maven Build')
        {
            steps{
                bat 'mvn clean install'
            }
        }
        stage('static code anaysis')
        {
            steps{
                    script{
                        withSonarQubeEnv(credentialsId: 'sonar-api') {
                           bat "mvn clean package sonar:sonar"
                        }
                    }
                }
        }
        stage('Code Quality Check')
        {
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
                }
            }
        }
        stage('NEXUS Integration')
        {
            steps{
                script{

                def readMaven = readMavenPom file: 'pom.xml'
                def nexurepo = readMaven.version.endsWith("SNAPSHOT") ? "app-snapshot" : "app-release"
                    nexusArtifactUploader artifacts: [
                    [artifactId: 'springboot',
                    classifier: '',
                    file: 'target/Uber.jar',
                    type: 'jar']],
                    credentialsId: 'nexus-auth',
                    groupId: 'com.example',
                    nexusUrl: '18.233.7.25:8081',
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    repository: nexurepo,
                    version: "${readMaven.version}"
                }
            }
        }




    }
}