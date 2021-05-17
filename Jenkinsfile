// vi:set nu ai ap aw smd showmatch tabstop=4 shiftwidth=4: 

def printCausesRecursively(cause) {
  if (cause.class.toString().contains("UpstreamCause")) {
    println "This job was caused by " + cause.toString()
	for (upCause in cause.upstreamCauses) {
	  printCausesRecursively(upCause)
	}
  } else {
    println "Root cause : " + cause.toString()
  }
}
    

pipeline {
  agent any
  options {
    timestamps();
    copyArtifactPermission('toprepo');
  }
  parameters {
    string (
        description: 'Tag Override Value',
        name: 'tagovr',
        defaultValue: ''
    )
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
	    script {
		  for (cause in manager.build.causes)
		  {
		    printCausesRecursively(cause)
		  }
		}    
      }	
	}
    
    stage('tag') {
        steps {
            script {
                if ( params.tagovr != '' ) {
                    def TAGSTRING = "${params.tagovr}"
                    echo "##### Tag OVERRIDE is applied"
                    echo "##### Tag: $TAGSTRING"
                    sh "make -e STAG=$TAGSTRING stag"
                } else {
                    sh 'make stag'
                }
            }
        }
    }

    stage('tag push') {
      steps {
        sh """
          make stagpush
        """
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
