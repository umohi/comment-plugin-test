pipeline {
   agent { docker { image 'maven:3.3.3' } }
   stages {
       stage('build') {
            steps {
                sh 'env'
		script {
		       myVar = sh (script: './ci/lookup-test-suite test_suite1', returnStdout: true).trim()
		}
		echo "${myVar}"
            }
        }
    }
}

