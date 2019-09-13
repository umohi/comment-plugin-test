/**
 * Libraries contain reusabile snippets that can be reused across multiple projects.
 */
@Library('bf-jenkins-utils@develop')_


def runExtraTestSuites = false
node('github-pr-builder') {
    testsuite = env.testsuite
    if (testsuite != null) {
        if (! validateTestSuiteName(testsuite)) {
          pullRequest.comment("Jenkins Master: ${testsuite} is not a recognized testsuite")
          currentBuild.result = 'UNSTABLE'
        }
        else {
          runExtraTestSuites = true
        }
    }

} //node


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
                    parallel getTestSuiteSteps(testsuite)
                  }
                }
            }
        }
    }// stages
} //pipeline

