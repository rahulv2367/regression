pipeline {
    agent any

    stages {
        stage('checkout') {
            steps {
                // Your build steps here
                git url: 'https://github.com/rahulv2367/regression.git', branch: 'main'
            }
        }
        stage('Test') {
            steps {
                // Run your tests and generate JUnit test result XML files
                script {
                    sh 'ls'
		    sh 'java -jar MAGiX_installation/cmdlineMAGiX.jar -e  EnvironmentDetails.properties -u MAGiX_installation/TestArtefacts -testsuite "AWS" -ReportsDestination local -headless -PassPercentage 80 -p'
                }
            }
        }
    }
     post {
        always {
            publishHTML ([
	            allowMissing: false,
	            alwaysLinkToLastBuild: false,
	            keepAll: false,
	            reportDir: 'htmlreports',
	            reportFiles: 'index.html',
	            reportName: 'HTML Report',
	            reportTitles: '',
	            useWrapperFileDirectly: true
            ])
        }
     }
} 

