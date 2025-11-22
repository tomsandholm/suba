// vi:set nu ai ap aw smd showmatch tabstop=4 shiftwidth=4: 

pipeline {
  agent { 
	label 'builder-amd64'
  }
  options {
    timestamps();
    copyArtifactPermission('toprepo');
  }

  parameters {
    string(
	  defaultValue: '',
	  name: 'DCO_TAG',
	  trim: true
	)
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
	    script {
		  writeFile(file: "./dco-tag1", text: "$DCO_TAG")
		}
        sh """
	  echo "workspace: $workspace"
	  cd $workspace
	  echo "directory listing:"
	  ls -l
	  git submodule update --recursive --init
	  ls -l subaa
	  cat subaa/data
	  echo "hostname: "
	  hostname
	  id
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
		  echo "Workspace: ${WORKSPACE}"
		  echo 'this is a test'
		  echo "DCO_TAG is ${env.DCO_TAG}"
      }	
	}
  }
  post {
    always {
	  archiveArtifacts artifacts: 'sub*.deb, workspace.tgz', onlyIfSuccessful: true
	  script {
	    echo "##### DCO_TAG: ${DCO_TAG}"
	  }
    }
  }
}
