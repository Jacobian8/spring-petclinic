def count = 0;
def lastSuccess = null;
def potentiallyBad = null; 

pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script{
                    potentiallyBad = sh (script: "git log -n 1 --pretty=format:'%H'", returnStdout: true)
                    if (lastSuccess != null) {
                        if (count == 8) {
                            count = 1
                            sh 'mvn clean install'
                        } else {
                            count += 1
                            return
                        }
                    } else {
                        sh 'mvn clean install'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                sh './mvnw test'
            }
        }

        stage('Package') {
            steps {
                sh './mvnw package'
            }
        }

        stage('Deploy') {
            steps {
                sh './mvnw deploy -DaltDeploymentRepository=internal.repo::default::file:///Users/jacobguirguis/Documents/Concordia/6_Winter_2020/SOEN_345/Assignments/Assignment_6/jenkins'
            }
        }
    }
    post {
		  success {
			  steps{
		  	  	slackSend (color: '#00FF00', message: "Build Succeeded: ${env.JOB_NAME} [${env.BUILD_NUMBER}]")
				  script{
					  lastSuccess = sh (script: "git log -n 1 --pretty=format:'%H'", returnStdout: true)	
				  }
			  }
		  }

		  failure {
			  steps{
				  //slackSend (color: '#FF0000', message: "Buil failed: ${env.JOB_NAME} [${env.BUILD_NUMBER}]")
				  sh "git bisect start ${potentiallyBad} ${lastSuccess}"
				  sh "git bisect run mvn clean test"
				  sh "git bisect reset"
			  }
		  }
	  }
}
