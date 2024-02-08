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
		
		stage ("Creating docker image") {
			steps {
				sh 'sudo docker build -t java-app:$BUILD_TAG .'
				sh 'sudo docker tag java-app:$BUILD_TAG jugnupanchal/java-app:$BUILD_TAG'
			}
		}

		stage ("Push on Docker Hub") {
			steps {
				withCredentials([string(credentialsId: 'docker_pass_id', variable: 'docker_hub_pass_var')]) {
				sh 'sudo docker login -u jugnupanchal -p ${docker_hub_pass_var}'
				sh 'sudo docker push jugnupanchal/java-app:$BUILD_TAG'
				}
			}
		}

		stage ("Testing the build") {
			steps {
				sh 'sudo docker run -dit --name java-test:$BUILD_TAG -p 8081:8080 jugnupanchal/java-app:$BUILD_TAG'
			}
		}

		stage ("QAT testing") {
			steps {
				sh 'sudo curl --silent http://3.110.168.117:8080/java-web-app'
			}
		}

	}
}

