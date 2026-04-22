pipeline {
    agent {
        node {
            label 'maven'
        }
    }

    environment {
        PATH = "/opt/apache-maven-3.9.15/bin:${env.PATH}"
    }

    stages {
        stage('Build') {
            steps {
                echo "--------------- Build Started ------------------------"
                sh 'mvn clean deploy -Dmaven.test.skip=true' 
                 echo "-------------- Build Completed ---------------------"
            }
        }

        stage( 'Test') {
            steps {
                echo "------------- Unit test Started ---------------------"
                sh 'mvn surefire-report:report'
                 echo "----------------- Unit test Completed --------------"
            }
        }
        stage('SonarQube Analysis') {
            environment {
                scannerHome = tool 'stalin-sonar-scanner'
            }
            steps {
                withSonarQubeEnv('stalin-sonarqube-server') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
    }
}

          
    
    
