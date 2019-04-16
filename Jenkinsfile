def bucket = 'deployment-packages-test1'
def functionName = 'Fibonacci'
def region = 'ap-southeast-2'

node {

	environment {
		DOTNET_PATH = "$PATH:/home/ec2-user/dotnet"
	}
	
    stage('Checkout'){
        checkout scm
    }
	
	stage('Restore Packages') {
		echo "DOTNET_PATH is: $DOTNET_PATH"
		sh '$(DOTNET_PATH)/dotnet restore'
//		sh "dotnet build src/HelloWorldJenkins"
		
	}
}