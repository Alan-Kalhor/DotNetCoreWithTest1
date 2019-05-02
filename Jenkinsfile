def bucket = 'deployment-packages-test1'
def functionName = 'Fibonacci'
def region = 'ap-southeast-2'

node {

	//environment {
//		DOTNET_ROOT="$HOME/dotnet"
//	}
	
	def DOTNET_PATH = '/home/ec2-user/dotnet'
	def FUNCTION_NAME = 'dotnettest2-4'
	def REGION = 'ap-southeast-2'
	def PROD_ALIAS = 'production'
	
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
	
	stage('Test') {
		sh "sudo $DOTNET_PATH/dotnet test"
	}
	
	stage('Deploy') {
		env.DOTNET_ROOT = "/home/ec2-user/dotnet"
		env.PATH = "$PATH:/home/ec2-user/dotnet"
		//sh "printenv | sort"
		sh "$DOTNET_PATH/dotnet-lambda list-functions"
		//dir("AWSServerlessWithTest2") {
		//	sh "$DOTNET_PATH/dotnet-lambda package --configuration release --framework netcoreapp2.1 --output-package bin/release/netcoreapp2.1/deploy-package.zip"
		//}
		dir("AWSServerlessWithTest2") {
			//sh "$DOTNET_PATH/dotnet-lambda deploy-function DotNetCoreWithTest1 --function-role JenkinsBuildRole"
			sh "$DOTNET_PATH/dotnet-lambda deploy-function --function-runtime dotnetcore2.1 --function-name dotnettest2-4  --function-memory-size 256 --function-timeout 30 --function-role mydotnetroll --function-handler AWSServerlessWithTest2::AWSServerlessWithTest2.LambdaEntryPoint::FunctionHandlerAsync --disable-interactive true"
			
			sh "$DOTNET_PATH/dotnet-lambda deploy-serverless dotnettest2-4"

		}
	}
	
	if (env.BRANCH_NAME == 'master') {
		stage('Publish') {
			def lambdaVersion = sh(
				script: "aws lambda publish-version --function-name ${FUNCTION_NAME} --region ${REGION} | jq -r '.Version'",
				returnStdout: true
			)
			sh "echo $lambdaVersion"

			def existing_aliases = sh(
				script: "aws lambda list-aliases --function-name ${FUNCTION_NAME} --region ${REGION} | jq -r '.Aliases[] | {Name: .Name}'",
				returnStdout: true
			)		
			
			sh "echo $existing_aliases"
			
			def prod_alias = sh(
				script: "aws lambda list-aliases --function-name ${FUNCTION_NAME} --region ${REGION} | jq -r '.Aliases[] | select(.Name == ${PROD_ALIAS}) | .Name'",
				returnStdout: true
			)
			
			sh "echo $prod_alias"
			
			//sh "aws lambda update-alias --function-name ${FUNCTION_NAME} --name production --region ${REGION} --function-version ${lambdaVersion}"
		}
	}	
	
	/*stage('Deploy Serverless') {
		env.DOTNET_ROOT = "/home/ec2-user/dotnet"
		env.PATH = "$PATH:/home/ec2-user/dotnet"

		dir("AWSServerlessWithTest2") {
			sh "$DOTNET_PATH/dotnet-lambda deploy-serverless dotnettest2-4"
		}
	}*/
	
}
