pipeline {
	agent {
		docker {
		    image 'node:lts-buster-slim' 
		    args '-p 3000:3000' 
		}
	}
	environment {
		CI = 'true'
	}
	stages {
		stage('Build') { 
		    steps {
			sh 'npm install' 
		    }
		}
		stage('Test') {
		    steps {
			sh './jenkins/scripts/test.sh'
		    }
		}
		stage('OWASP DependencyCheck') {
		    steps {
			dependencyCheck additionalArguments: '--format HTML --format XML', odcInstallation: 'OWASP-DC'
		    }
		}
		stage('Deliver') {
		    steps {
			sh './jenkins/scripts/deliver.sh'
			input message: 'Finished using the web site? (Click "Proceed" to continue)'
			sh './jenkins/scripts/kill.sh'
		    }
		}

	}
	post {
		success {
		    dependencyCheckPublisher pattern: 'dependency-check-report.xml'
		}
    	}
}
