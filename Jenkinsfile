/**
 * Libraries contain reusabile snippets that can be reused across multiple projects.
 */
@Library('bf-jenkins-utils@develop')_


def runExtraTestSuites = false
node('github-pr-builder') {
    testsuite = env.testsuite
    if (testsuite != null) {
        if (! validateTestSuiteName(testsuite)) {
          pullRequest.comment("Jenkins: ${testsuite} is not a recognized testsuite")
          println "Jenkins: ${testsuite} is not a recognized testsuite"
          currentBuild.result = 'UNSTABLE'
        }
        else {
          runExtraTestSuites = true
          println "Jenkins: recognized VALID testsuite"
        }
    }

  docker.withRegistry('https://artifacts.barefootnetworks.com:9444', 'nexus-docker-creds') {
      docker.image("artifacts.barefootnetworks.com:9444/bf/p4factory:master").inside('--privileged --cap-add=ALL  --user=root') {
        sh 'cd /sandals; git fetch ; git checkout DEV-269-testsuite-framework'
        myVar = sh (script: "/sandals/sde-test/docker/testsuite-generator.py ${env.testsuite}", returnStdout: true).trim()
        myArr = myVar.tokenize()
      }


  } // docker

} //node

def extraTestStages = [failFast: true]
  for (int i = 0; i < myArr.size(); i++) {
    def vmNumber = i //alias the loop variable to refer it in the closure
    script_name = "${myArr[i]}"
    extraTestStages["Running Testsuite ${myArr[i]}"] = {
      stage("Running Tests Script ${myArr[i]}") {
        sh 'hostname; cd /sandals; sleep $(shuf -i 10-39 -n 1); git fetch ; git checkout DEV-269-testsuite-framework; cd -'
        sh "/sandals/sde-test/docker/testsuite-generator.py"
        sh "BRANCH=master /sandals/sde-test/docker/${script_name}"
      }
    }
  }

pipeline {
   agent none
   stages {
       stage('parallel stage') {
           agent {
                    docker {
                        label 'github-pr-builder'
                        image "artifacts.barefootnetworks.com:9444/bf/p4factory:master"
                        registryUrl 'https://artifacts.barefootnetworks.com:9444'
                        registryCredentialsId 'nexus-docker-creds'
                        args '--privileged --cap-add=ALL  --user=root'
                    }
                }
            steps {
                script {
                  if (runExtraTestSuites == true) { 
                    getTestSuiteSteps(testsuite)
                  }
                }
            }
        }
    }// stages
} //pipeline

