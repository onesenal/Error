node {
  def app
  def dockerfile
  def anchorefile
	
  try {
    stage('Checkout') {
      // Clone the git repository
      checkout scm
      def path = sh returnStdout: true, script: "pwd"
      path = path.trim()
      dockerfile = path + "/Dockerfile"
      anchorefile = path + "/anchore_images"
    }
    
    stage('SonarQube analysis') {
        def scannerHome = tool 'sonarqube';
        withSonarQubeEnv('sonarserver'){
            sh "${scannerHome}/bin/sonar-scanner \
	      -Dsonar.projectKey=sonarqube \
	      -Dsonar.host.url=http://192.168.160.244:9000 \
	      -Dsonar.login=807e0f2bc82e3c377436e2b6292ed7bc73b04e24 \
	      -Dsonar.sources=. \
	      -Dsonar.report.export.path=sonar-report.json \
	      -Dsonar.exclusions=report/* \
	      -Dsonar.dependencyCheck.jsonReportPath=./report/dependency-check-report.json \
	      -Dsonar.dependencyCheck.xmlReportPath=./report/dependency-check-report.xml \
	      -Dsonar.dependencyCheck.htmlReportPath=./report/dependency-check-report.html"
        }
    }
    }
}
