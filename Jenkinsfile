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
        sh '''./mvnw sonar:sonar \\
  -Dsonar.projectKey=Petclinic \\
  -Dsonar.host.url=http://172.31.27.79:9000 \\
  -Dsonar.login=sqp_6f291a8147341749851fd35a92d51b2885f1e54a'''
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
      }
    }

    stage('Deploy') {
      parallel {
        stage('Deploy') {
          steps {
            sh './mvnw spring-boot:run </dev/null &>/dev/null &'
          }
        }

        stage('Integration and Performance Tests ') {
          agent {
            node {
              label 'test'
            }

          }
          steps {
            sh './mvnw verify'
            junit '**/target/surefire-reports/'
          }
        }

      }
    }

  }
}