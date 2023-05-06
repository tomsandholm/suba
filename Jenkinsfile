// vi:set nu ai ap aw smd showmatch tabstop=4 shiftwidth=4: 

pipeline {
	agent { 
		label 'builder-arm32v7'
	}
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
        cleanWs()
	checkout scm
      }
    }

    stage('setup') {
      steps {
        sh """
	  echo "workspace: $workspace"
	  cd $workspace
	  echo "directory listing:"
	  ls -l
	  echo "hostname: "
	  hostname
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
	  whoami
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
