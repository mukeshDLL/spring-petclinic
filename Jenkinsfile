pipeline {
  agent {
    node {
      label 'msbcmutest'
    }

  }
  stages {
    stage('compile') {
      steps {
        sh './mvnw clean compile'
      }
    }

    stage('Static Analysis') {
      steps {
        sh '''./mvnw sonar:sonar \\
  -Dsonar.host.url=http://172.31.87.96:9000 \\
  -Dsonar.projectKey=Petclinic \\
  -Dsonar.login=sqp_192933768eed9e396a85743b827d808dc91ef0a0'''
      }
    }

    stage('Unit Test') {
      steps {
        sh './mvnw "-Dtest=**/petclinic/*/*.java" test'
      }
    }

    stage('Package') {
      steps {
        sh './mvnw package -DskipTests=true'
        sh './mvnw package -DskipTests=true'
      }
    }

    stage('Deploy') {
      parallel {
        stage('Deploy') {
          steps {
            sh './mvnw spring-boot:run </dev/null &>/dev/null &'
          }
        }

        stage('Integration and Performance') {
          steps {
            sh './mvnw verify'
          }
        }

        stage('Archive JUnit formatted test results') {
          steps {
            junit '**/target/surefire-reports/'
          }
        }

      }
    }

  }
}