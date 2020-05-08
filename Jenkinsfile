pipeline {
    agent {
    	label 'homelab'
    }
    environment {
        BUCKET = 'noahing-com'
    }
    stages {
        stage('Activate Service Account') {
	    when {
        	expression { BRANCH_NAME ==~ /master/ }
      	    }
            steps{
            	withCredentials([file
		(credentialsId: 'homelab-gcp-jenkins-service-account', 
		variable: 'GC_KEY')]) {
    	    	sh("gcloud auth activate-service-account --key-file=${GC_KEY}")}
        	}
    	    }
        stage('Upload to GCS') {
	    when {
        	expression { BRANCH_NAME ==~ /master/ }
      	    }
            steps{
	    	sh "gsutil cp -r ./* gs://${env.BUCKET}"
        	}
    	    }
    	}
    post {
        always {
            deleteDir() /* clean up our workspace */
        }
        success {
            echo 'Success'
        }
        failure {
            echo 'Failure'
        }
       }
}
