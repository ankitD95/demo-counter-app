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
                sh 'mvn test'
            }
        }



    }
}