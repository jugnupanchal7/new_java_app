pipeline {
	agent {
		label "jenkins-Slave"
	}
	stages {
		stage ("Pulling the code from SCM") {
			steps {
				git branch: 'main', url: 'https://github.com/jugnupanchal7/new_java_app.git'			
			}
		}

		stage ("Installing git") {
			steps {
				sh 'sudo yum install -y git'
			}
		}

		stage ("Build the code") {
			steps {
				sh 'sudo mvn dependency:purge-local-repository'
			}
		}

		
	}
}

