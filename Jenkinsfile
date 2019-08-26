node('master') {
 stage('Determine Target Branch') {
  // The 'CHANGE_TARGET' env var is set by Jenkins and contains the value of the
  // branch the PR will be merged into if everything works well. We use this
  // variable to decide which p4factory docker image to use
  if (env.CHANGE_TARGET == null) {
     env.CHANGE_TARGET = "master"
  }

  target_branch = "${env.CHANGE_TARGET}"

  // for now we will only build master branch
  if (target_branch =~ /master/) {
    skip_build = false
    println "Jenkinss: targt branch -- ${target_branch}"
//    pullRequest.comment("Jenkinss: targt branch -- ${target_branch}")
  }
  else {
    skip_build = true
    println "Jenkinss: targt branch -- ${target_branch}"
//    pullRequest.comment("Jenkinss: targt branch -- ${target_branch}")
  }
 }
}

pipeline {
   agent { docker { image 'maven:3.3.3' } }
   stages {
       stage('build') {
            steps {
                sh 'env'
            }
        }
    }
}

