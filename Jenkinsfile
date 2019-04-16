def bucket = 'deployment-packages-test1'
def functionName = 'Fibonacci'
def region = 'ap-southeast-2'

node {

	environment {
		PATH = "$PATH:/home/ec2-user/dotnet"
	}
	
    stage('Checkout'){
        checkout scm
    }
	
	stage('Restore Packages') {
		echo "PATH is: $PATH"
		sh 'dotnet restore'
//		sh "dotnet build src/HelloWorldJenkins"
		
	}
}