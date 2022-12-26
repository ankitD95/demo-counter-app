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
                sh 'mvn test'
            }
        }
        stage('INTEGRATION Test')
        {
            steps{
                sh 'mvn verify -DskipUnitTests'
            }
        }
        stage('Maven Build')
        {
            steps{
                sh 'mvn clean install'
            }
        }
        stage("Sonarqube Analysis")
        {
            steps{
                withSonarQubeEnv(credentialsId: 'sonar-api') {
                   bat "mvn clean package sonar:sonar"
                }
            }
        }



    }
}