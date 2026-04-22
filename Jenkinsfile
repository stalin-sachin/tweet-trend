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

        stage("Quality Gate") {
            steps {
                script {
                    
                    timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
                      def qg = waitForQualityGate()  // Reuse taskId previously collected by withSonarQubeEnv
                      if (qg.status != 'OK') {
                          echo "Quality Gate failed: ${qg.status}, but continuing..."
                      }
                    }
                }      
            }
        }
    }
}

          
    
    
