properties([pipelineTriggers([githubPush()])])

node('linux') {   
	stage('Test') {    
		git credentialsId: '3fe3f489-a36f-4b06-936d-561fa40f4146', url: 'https://github.com/buhr3940/java-project2.git'
		sh 'ant -f test.xml -v'
		junit 'reports/*.xml'
	}   
	stage('Build') {    
		sh 'ant -f build.xml -v'   
	}   
	stage('Deploy') {    
		sh 'aws s3 cp *.jar s3://buhr3940-pipeline-bucket/index.html'   
	}
	stage('Report') { 
		withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'buhr3940', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
    
			sh 'aws cloudformation describe-stack-resources --region us-east-1 --stack-name jenkins-stack'
		}   
	}
}
