pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh './mvnw clean install'
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
                slackSend(color: 'good', message: "Build Succeeded")
            }
        }
  }
}
