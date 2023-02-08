 node {
    stage('Checkout') {
      // Clone the git repository
      checkout scm
      def path = sh returnStdout: true, script: "pwd"
      path = path.trim()
      dockerfile = path + "/Dockerfile"
    }
    stage('OWASP Dependency-Check Vulnerabilities ') {
    dependencyCheck additionalArguments: '''
	    -s "." 
	    -f "ALL"
	    -o "./report/"
	    --prettyPrint
	    --disableYarnAudit''', odcInstallation: 'OWASP Dependency-check'
	    dependencyCheckPublisher pattern: 'report/dependency-check-report.xml'
    }
    stage('SonarQube analysis') {
        def scannerHome = tool 'sonarqube';
        withSonarQubeEnv('sonarserver'){
            sh "${scannerHome}/bin/sonar-scanner \
	      -Dsonar.projectKey=innogrid \
	      -Dsonar.host.url=http://192.168.160.229:9000 \
	      -Dsonar.login=0c50fc8e6a4e1a3a7a5104f0f045367b354fbd0a \
	      -Dsonar.sources=. \
	      -Dsonar.report.export.path=sonar-report.json \
	      -Dsonar.exclusions=report/* \
	      -Dsonar.dependencyCheck.jsonReportPath=./report/dependency-check-report.json \
	      -Dsonar.dependencyCheck.xmlReportPath=./report/dependency-check-report.xml \
	      -Dsonar.dependencyCheck.htmlReportPath=./report/dependency-check-report.html"
        }
    }
}
