def bucket = 'deployment-packages-test1'
def functionName = 'Fibonacci'
def region = 'ap-southeast-2'

node {

	environment {
		DOTNET_PATH = '"/home/ec2-user/dotnet'
	}
	
    stage('Checkout'){
        checkout scm
    }
	
//	stage('Restore Packages') {
//		sh "printenv | sort"
//		sh '/home/ec2-user/dotnet/dotnet restore'
//		sh "dotnet build src/HelloWorldJenkins"		
//	}
	
    stage('Check'){
		sh "./dotnet --version"
    }
	
	stage('Build') {
		//sh '/home/ec2-user/dotnet/dotnet build --configuration Release'
		dir ('/home/ec2-user/dotnet/') { 
			sh('dotnet build --configuration Release')
		}		
	}
}