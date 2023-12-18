// vi:set nu ai ap aw smd showmatch tabstop=4 shiftwidth=4: 

@Library('tom-sand') _

pipeline {
	agent { 
		label 'builder-amd64'
	}
  options {
    timestamps();
    copyArtifactPermission('toprepo');
  }

  environment {
    CAUSE = "${currentBuild.getBuildCauses()[0].shortDescription}"
	DCO_TAG = "nothing"
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
	    script {
		  DCO_TAG = "test-tag"
		  writeFile("./dco-tag","$DCO_TAG")
		}
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
		  echo "DCO_TAG is ${env.DCO_TAG}"
      }	
	}
  }
  post {
    always {
      archiveArtifacts artifacts: 'sub*.deb', onlyIfSuccessful: true
	  script {
	    echo "##### DCO_TAG: ${DCO_TAG}"
		dco_tag()
	  }
    }
  }
}
