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
		
		stage ("Creating docker Image") {
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

		stage ("Testing the Build") {
			steps {
				sh 'sudo docker run -dit --name java-test$BUILD_TAG -p 8082:8080 jugnupanchal/java-app:$BUILD_TAG'
			}
		}

		stage ("QAT testing") {
			steps {
				sh 'sudo curl --silent http://3.108.64.245:8082/java-web-app/'
			}
		}

		stage ("Approval status") {
				steps {
					script {
						 Boolean userInput = input(id: 'Proceed1', message: 'Do you want to Promote this build?', parameters: [[$class: 'BooleanParameterDefinition', defaultValue: false, description: '', name: 'Please confirm you agree with this']])
                				echo 'userInput: ' + userInput
					}
				}
			}

		stage ("Production ENV") {
			steps {
				sshagent(credentials:['sshagent-cred']) {
			    	 	sh "ssh -o StrictHostKeyChecking=no ubuntu@13.213.48.76 sudo docker run  -dit  -p  49153:8080  jugnupanchal/java-app:$BUILD_TAG"
				}
			}
		}


	}
}
