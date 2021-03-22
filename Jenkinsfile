pipeline {
  agent any
  options {
    timestamps();
  }

  stages {
    stage('checkout') {
      steps {
        checkout scm
      }
    }
    stage('build') {
      steps {
        sh """
          make all
        """
      }
    }
  }
}

