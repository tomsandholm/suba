// vi:set nu ai ap aw smd showmatch tabstop=4 shiftwidth=4: 

pipeline {
  agent any
  options {
    timestamps();
    copyArtifactPermission('toprepo');
  }

  environment {
    CAUSE = "${currentBuild.getBuildCauses()[0].shortDescription}"
  }

  stages {

    stage('checkout') {
      steps {
        checkout scm
      }
    }

    stage('setup') {
      steps {
        sh """
          autoreconf --verbose --install --force
          ./configure
        """
      }
    }

    stage('build') {
      steps {
        sh """
          make all
          make dist
          make package
        """
      }
    }

	stage('check parent') {
	  steps {
		  echo "Build caused by ${env.CAUSE}"
		  echo 'Build caused by ${env.CAUSE}'
		  echo "Workspace: ${WORKSPACE}"
		  echo 'Workspace: ${WORKSPACE}'
		  echo 'this is a test'
		  echo 'this is not a test'
      }	
	}
  }
  post {
    always {
      archiveArtifacts artifacts: 'sub*.deb', onlyIfSuccessful: true
      step([$class: 'WsCleanup'])
    }
  }
}
