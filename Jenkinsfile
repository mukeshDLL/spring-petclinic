pipeline {
  agent any
  stages {
    stage('compile') {
      steps {
        sh './mvnw clean compile'
      }
    }

    stage('Static Analysis') {
      steps {
        sh '''mvn clean verify sonar:sonar \\
  -Dsonar.projectKey=Petclinic \\
  -Dsonar.host.url=http://44.203.170.25:9000 \\
  -Dsonar.login=sqp_192933768eed9e396a85743b827d808dc91ef0a0'''
      }
    }

  }
}