/**
 * Libraries contain reusabile snippets that can be reused across multiple projects
 */
@Library('bf-jenkins-utils@master')_

 node('master') {
    pullRequest.comment("JIRA ID found in comments and/or PR title")

 } //node


pipeline {

  agent any

  stages {
    stage('Test') {
      when { changeRequest() }
      steps {
        script {
          echo "Current Pull Request ID: ${env.CHANGE_ID}"
        }
      }
    }
    stage('Test2') {
      steps {
        script {
          pullRequest.comment("Test")
        }
      }
    }
  }
}
