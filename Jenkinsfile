pipeline {
	agent {
		label "jenkins-Slave
	}
	stages {
		stage ("Pulling the code from SCM") {
			step {
				git branch: 'main', url: 'https://github.com/jugnupanchal7/new_java_app.git'			
			}
		}

		stage ("Build the code") {
			step {
				sh 'sudo mvn dependency:purge-local-repository'
				sh 'sudo mvn clean package'
			}
		}

		
	}
}

