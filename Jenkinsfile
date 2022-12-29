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




    }
}