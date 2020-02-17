stage 'CI'
node {
    
    notify('Started')
	
	stage 'Checkout'

	checkout scm
	
	// git 'https://github.com/gap401/jenkins2-course-spring-petclinic.git'

	bat label: '', script: 'mvn clean verify'

	stage 'Save Workspace'
		
	// stash code & dependencies to expedite subsequent testing
	// and ensure same code & dependencies are used throughout the pipeline
	// stash is a temporary archive
	stash name: 'everything', 
		  excludes: 'test-results/**', 
		  includes: '**'

	stage 'Publish Code Coverage'

	publishHTML([allowMissing: true, 
			alwaysLinkToLastBuild: false, 
			keepAll: true, 
			reportDir: 'target/site/jacoco/', 
			reportFiles: 'index.html', 
			reportName: 'Code Coverage Report', reportTitles: ''])

	stage 'Archive Test Results'
	
	step([$class: 'JUnitResultArchiver',
		testResults: 'target/surefire-reports/TEST-*.xml'])
	archiveArtifacts "target/*.?ar"

}


def notify(status){
    emailext (
      to: "gapmvc@gmail.com",
      subject: "${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """<p>${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
        <p>Check console output at <a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a></p>""",
    )
}
