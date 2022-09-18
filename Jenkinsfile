// vi:set nu ai ap aw smd showmatch tabstop=4 shiftwidth=4: 

pipeline {
	agent { label: 'builder' }
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
	  echo "workspace is $workspace"
	  cd $workspace
	  ls -l
          autoreconf --verbose --install --force
          ./configure
        """
      }
    }

    stage('build') {
      steps {
        sh """
	  pwd
	  ls -l
	  make clean
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
      }	
	}
  }
  post {
    always {
      archiveArtifacts artifacts: 'sub*.deb', onlyIfSuccessful: true
    }
  }
}
