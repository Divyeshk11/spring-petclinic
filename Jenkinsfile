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
  -Dsonar.host.url=http://65.2.55.72:9000 \\
  -Dsonar.login=sqp_6f291a8147341749851fd35a92d51b2885f1e54a'''
      }
    }

  }
}