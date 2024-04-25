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
//		    sh 'cp -r /home/jenkins/agent/workspace/magix_main/Reports /var/jenkins_home/jobs/magix/branches/main/htmlreports/'
		    sh 'java -jar MAGiX_installation/cmdlineMAGiX.jar -e  EnvironmentDetails.properties -u MAGiX_installation/TestArtefacts -testsuite "AWS" -ReportsDestination local -headless -PassPercentage 50 -p'
		    sh 'ls -ltrh'
		    sh 'pwd' 	
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
	            reportDir: '/var/jenkins_home/jobs/magix/branches/main/htmlreports',
	            reportFiles: 'index.html',
	            reportName: 'HTML Report',
	            reportTitles: '',
	            useWrapperFileDirectly: true
            ])
        }
     }
} 

