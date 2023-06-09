pipeline {
 agent any
 triggers {
   pollSCM '* * * * *'
 }	
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
	 	stage("Package"){
			steps{
				sh "./gradlew build"
			}
		} 
	 	stage("Docker build"){
			steps{
				sh "docker build -t leszko/calculator ."
			}
		}
	 	stage("Docker push"){
			steps{
				sh "docker push leszko/calculator"
			}
		} 
	 	stage("Deploy to staging"){
			steps{
				sh "docker run -d --rm -p 8765:8080 --name calculator leszko/calculator"
			}
		}
	 	stage("Acceptance test"){
			steps{
				sleep 60
				sh "chmod +x acceptance_test.sh && ./acceptance_test.sh"
			}
		}
	}
}
post {
	 always {
		 mail to: 'jamesxpower@gmail.com',
		 subject: "Completed Pipeline: ${currentBuild.fullDisplayName}",
		 body: "Your build completed, please check: ${env.BUILD_URL}"
		 sh "docker stop calculator"
	 }
}
