podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: jnlp
        image: 058264170697.dkr.ecr.ap-south-1.amazonaws.com/jenkins:agent-jdk21-v1
      - name: trivy
        image: 058264170697.dkr.ecr.ap-south-1.amazonaws.com/jenkins:trivy-0.49.1
        command:
        - sleep
        args:
        - 9999999
      - name: kaniko
        image: 058264170697.dkr.ecr.ap-south-1.amazonaws.com/jenkins:kaniko-debug
        command:
        - sleep
        args:
        - 9999999
        volumeMounts:
        - name: kaniko-secret
          mountPath: /kaniko/.docker
      serviceAccount: jenkins-admin
      restartPolicy: Never
      volumes:
      - name: kaniko-secret
        secret:
            secretName: regcred
            items:
            - key: .dockerconfigjson
              path: config.json
''') {

// properties([
//     parameters([
//         string(name: 'NAMESPACE', defaultValue: 'gems2', description: 'Kubernetes namespace'),
//         string(name: 'DEPLOYMENT_NAME', defaultValue: 'my-springboot-deployment', description: 'Application deployment name')
//     ])
// ])

node(POD_LABEL) {
    
    environment{
    REGISTRY = "058264170697.dkr.ecr.ap-south-1.amazonaws.com/jenkins"
    }
    
    stage('checkout SCM') {
      git url: 'https://git-codecommit.ap-south-1.amazonaws.com/v1/repos/test', branch: 'develop', credentialsId: 'codecommit'
      container('jnlp') {
          sh '''
            ls
          '''
      }
    }
    
    stage('Magix_Connection') {
      container('jnlp') {
        sh """
        cd MAGiX_installation
        mkdir -p reports
        java -jar cmdlineMAGiX.jar -e  EnvironmentDetails.properties -u TestArtefacts -testsuite "AWS" -ReportsDestination local -headless -PassPercentage 80 -p
        """

        // Publish JUnit test results
        junit '**/target/surefire-reports/*.xml'

        // Generate HTML report
        publishHTML(target: [
        allowMissing: false,
        alwaysLinkToLastBuild: true,
        keepAll: true,
        reportDir: 'reports',
        reportFiles: 'index.html',
        reportName: 'JUnit Test Report'
        ])
      }
    }    
  }
}
