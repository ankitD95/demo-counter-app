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



    }
}