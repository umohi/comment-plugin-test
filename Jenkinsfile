node('master') {
 stage('Test suite') {
	script {
	     myVar = sh (script: "./ci/lookup-test-suite ${env.testsuite}", returnStdout: true).trim()
	     myArr = myVar.split()
        }
 	echo "${myVar}"
	echo "test suite ------> ${env.testsuite}"
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
       stage('Test Suite Execution') {
            steps {
               script {
 	         for (int i = 0; i < myArr.length; i++) {
    	    	    stage("Test ${myVar[i]}") {
            	      echo "yo"
    	            }
	         }
               }
	    }
        }
    }// stages
} //pipeline

