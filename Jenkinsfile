pipeline {
 agent any
 stages {
		stage("Static code analysis") {
			 steps {
				sh "./gradlew checkstyleMain"
				publishHTML (target: [
					 reportDir: 'build/reports/checkstyle/',
					 reportFiles: 'main.html',
					 reportName: "Checkstyle Report"
				])				
			 }
		} 
		stage("Code coverage") {
			 steps {
				 sh "./gradlew jacocoTestReport"
				 publishHTML (target: [
					 reportDir: 'build/reports/jacoco/test/html',
					 reportFiles: 'index.html',
					 reportName: "JaCoCo Report"
				 ])				 
				 sh "./gradlew jacocoTestCoverageVerification"
			 }
		} 
		stage("Compile") {
			 steps {
				sh "./gradlew compileJava"
			 }
		}
		stage("Unit test") {
			 steps {
				sh "./gradlew test"
			}
		}
	}
}