def bucket = 'deployment-packages-test1'
def functionName = 'Fibonacci'
def region = 'ap-southeast-2'

node {

	//environment {
//		DOTNET_ROOT="$HOME/dotnet"
//	}
	
	def DOTNET_PATH = '/home/ec2-user/dotnet'
	//def DOTNET_LAMBDA_PATH = '/home/ec2-user/.dotnet/tools'

	
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
		env.DOTNET_ROOT = "/home/ec2-user/dotnet"
		env.PATH = "$PATH:/home/ec2-user/dotnet"
		//sh "printenv | sort"
		//sh "sudo $DOTNET_LAMBDA_PATH/dotnet-lambda help"
		//sh "sudo $DOTNET_PATH/dotnet tool install -g Amazon.Lambda.Tools"
		//sh "sudo $DOTNET_PATH/dotnet tool uninstall -g Amazon.Lambda.Tools"
		sh "$DOTNET_PATH/dotnet-lambda"
	}
}
