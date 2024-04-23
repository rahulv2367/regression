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
                    sh 'cd MAGiX_installation'
		    sh 'java -jar cmdlineMAGiX.jar -e  EnvironmentDetails.properties -u TestArtefacts -testsuite "AWS" -ReportsDestination local -headless -PassPercentage 80 -p'
                }
            }
        }
    }

    post {
        always {
            // Copy the HTML report to the Jenkins controller
            script {
                sh 'cp -r /path/to/html/report $JENKINS_HOME/html_report'
            }

            // Publish HTML report from Jenkins controller
            script {
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: '$JENKINS_HOME/html_report',
                    reportFiles: 'index.html',
                    reportName: 'JUnit Test Report'
                ])
            }
        }
    }
}
