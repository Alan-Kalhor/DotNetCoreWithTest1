def bucket = 'deployment-packages-test1'
def functionName = 'Fibonacci'
def region = 'ap-southeast-2'

node {

	//environment {
	//	DOTNET_PATH = '/home/ec2-user/dotnet'
	//}
	
	def DOTNET_PATH = '/home/ec2-user/dotnet'

	
    stage('Checkout'){
        checkout scm
    }
	
//	stage('Restore Packages') {
//		sh "printenv | sort"
//		sh '/home/ec2-user/dotnet/dotnet restore'
//		sh "dotnet build src/HelloWorldJenkins"		
//	}
/*	
    stage('Check'){
		sh "sudo -s chmod +x /home/ec2-user/dotnet"
	
		sh "echo $DOTNET_PATH"
		

		//sh "sudo /home/ec2-user/dotnet/dotnet --version"
		sh "sudo $DOTNET_PATH/dotnet --version"
		
		//dir('/home/ec2-user/dotnet'){
		  //sh 'cdr=$(pwd); $cdr/jenkins.sh "dotnet --version"'
		 // sh "sudo dotnet --version"
		//}		
    }
*/	
	stage('Clean') {
		sh "sudo $DOTNET_PATH/dotnet clean"
	}
	
	stage('Build') {
		sh "sudo $DOTNET_PATH/dotnet build --configuration Release"
	}
	
	stage('Deploy') {
		sh "sudo $DOTNET_PATH/dotnet lambda help"
	}
}