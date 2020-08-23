
//@Library('mpl') _

//MPLPipeline {}

pipeline {
  agent any //{ label 'labelname' }

  options {
    timeout(time: 60, unit: 'MINUTES')
    timestamps()
    buildDiscarder(logRotator(numToKeepStr: '2'))
  }
  
 environment {
 
     BUILDSERVER_WORKSPACE="${WORKSPACE}"
     BUILD_NO=sh(returnStdout:true,script:"echo ${GIT_BRANCH}_${BUILD_NUMBER} | sed 's/\\//_/g'").trim()
     
 }
  stages {
  
   stage('Server Ready?') {
  
      steps {
       echo "Server is ready"
      script{
      br_name=sh(script:"echo ${BRANCH_NAME}|tr '/' '_' ",returnStdout:true).trim()
      def buildid=env.BUILD_ID
      echo buildid
      currentBuild.displayName="#"+env.BUILD_ID
		}
       sh 'git clean -dfx'
      }
    }

    stage('Clean Workspace') {
      steps {
        // Clean the workspace
       script{
        sh 'mvn clean'
        echo "Cleaning Workspace Done"
        }
       }

      } 
      stage('Check Out') {
      steps {
        // Clean the workspace
       script{
       // sh 'mvn clean'
        echo "Cleaning Workspace Done"
        }
       }

      }   
      
    stage('Build') {
      steps {
        // build, build stages can be made in parallel aswell
        // build stage can call other stages
        // can trigger other jenkins pipelines and copy artifact from that pipeline
        script{
        sh 'mvn install'
        echo "Build Done"
        }
      }
    }

    stage('Test') {
      steps {
        // Test (Unit test / Automation test(Selenium/Robot framework) / etc.)
      }
    }

    stage('Code Analysis') {
      steps {
        // Static Code analysis (Coverity/ SonarQube /openvas/Nessus etc.)
      }
    }

    stage('Generate Release Notes') {
      steps {
        // Release note generation .
      }
    }

    stage('Tagging') {
      steps {
        // Tagging specific version number
      }
    }

    stage('Release') {
      steps {
        // release specific versions(Snapshot / release / etc.)
      }
    }

    stage('Deploy') {
      steps {
        // Deploy to cloud providers /local drives /artifactory etc.
        // Deploy to Deploy/prod /test/ etc
      }
    }
  }

  post {
    success {
      echo "SUCCESS"
    }
    failure {
      echo "FAILURE"
    }
    changed {
      echo "Status Changed: [From: $currentBuild.previousBuild.result, To: $currentBuild.result]"
    }
    always {
      script {
        def result = currentBuild.result
        if (result == null) {
          result = "SUCCESS"
        }
      }
    }
  }
}
