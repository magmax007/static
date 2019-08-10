pipeline {
	agent any
	stages {
		stage('Lint HTML') {
			steps {
				sh 'tidy -q -e *.html'
			}
		}
		stage('Upload to AWS') {
			steps {
				withAWS(region:'us-east-2',credentials:'aws-static') {
    					s3Upload(pathStyleAccessEnabled:true, payloadSigningEnabled: true, file:'index.html', bucket:'pipelineudacityproject4')
				}
			}
		}
		stage('Check Website') {
			steps {
				timeout(time: 1, unit: 'MINUTES') {
					retry(3) {
						sh 'curl -Is https://pipelineudacityproject4.s3.us-east-2.amazonaws.com/index.html  | head -1'
						}		
					}
				}
			}
		}
}

