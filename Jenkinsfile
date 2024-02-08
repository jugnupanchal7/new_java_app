pipeline {
	agent {
		label "Ubuntu-Slave"
	}
	stages {
		stage ("Pulling the code from SCM") {
			steps {
				git branch: 'main', url: 'https://github.com/jugnupanchal7/new_java_app.git'			
			}
		}

		stage ("Build the code") {
			steps {
				sh 'sudo mvn dependency:purge-local-repository'
				sh 'sudo mvn clean package'
			}
		}

		
	}
}

