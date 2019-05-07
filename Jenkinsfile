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
	
	def lambdaVersion = ''
	
    stage('Checkout'){
        checkout scm
    }
	
	stage('Build') {
		sh "sudo $DOTNET_PATH/dotnet clean"	
		sh "sudo $DOTNET_PATH/dotnet build --configuration Release"
	}
	
	stage('Test') {
		sh "sudo $DOTNET_PATH/dotnet test"
	}
	
/*
	stage('Deploy') {
		env.DOTNET_ROOT = "/home/ec2-user/dotnet"
		env.PATH = "$PATH:/home/ec2-user/dotnet"

		sh "$DOTNET_PATH/dotnet-lambda list-functions"
		dir("AWSServerlessWithTest2") {
			//sh "$DOTNET_PATH/dotnet-lambda deploy-function DotNetCoreWithTest1 --function-role JenkinsBuildRole"
			sh "$DOTNET_PATH/dotnet-lambda deploy-function --function-runtime dotnetcore2.1 --function-name ${FUNCTION_NAME}  --function-memory-size 256 --function-timeout 30 --function-role mydotnetroll --function-handler AWSServerlessWithTest2::AWSServerlessWithTest2.LambdaEntryPoint::FunctionHandlerAsync --disable-interactive true"
			
			sh "$DOTNET_PATH/dotnet-lambda deploy-serverless ${FUNCTION_NAME}"

		}
	}
*/
	
	if (env.BRANCH_NAME == 'master') {
		stage('Publish') {
		
			lambdaVersion = sh(
				script: "aws lambda publish-version --function-name ${FUNCTION_NAME} --region ${REGION} | jq -r '.Version'",
				returnStdout: true
			)
			sh "echo $lambdaVersion"
			
/*
			def existing_aliases = sh(
				script: "aws lambda list-aliases --function-name ${FUNCTION_NAME} --region ${REGION} | jq -r '.Aliases[]'",
				returnStdout: true
			)		

			sh "echo $existing_aliases"
*/			
			
			def is_prod_alias_exists = sh(
				script: "aws lambda list-aliases --function-name ${FUNCTION_NAME} --region ${REGION} | jq -r '.Aliases[] | select(.Name == \"production\") | 1'",
				returnStdout: true
			)
			
			//sh "echo $is_prod_alias_exists"
			
			if (is_prod_alias_exists == 1) {
				sh "echo 'exists'"
			}
			else {
				sh "echo 'not exists'"
			}
			
			//sh "aws lambda update-alias --function-name ${FUNCTION_NAME} --name production --region ${REGION} --function-version ${lambdaVersion}"
		}


		stage('Deploy to Staging?') {
			milestone 2
			input "Do you want to deploy to Staging?"
		}

		stage('Deploy to Staging') {
			sh "aws lambda update-alias --function-name ${FUNCTION_NAME} --name staging --region ${REGION} --function-version ${lambdaVersion}"
		}
		
		stage('Deploy to Production?') {
			milestone 3
			input "Do you want to deploy to Production?"
		}		
		
		stage('Deploy to Production') {
			sh "aws lambda update-alias --function-name ${FUNCTION_NAME} --name production --region ${REGION} --function-version ${lambdaVersion}"
		}
	}	
	

	/*stage('Deploy Serverless') {
		env.DOTNET_ROOT = "/home/ec2-user/dotnet"
		env.PATH = "$PATH:/home/ec2-user/dotnet"

		dir("AWSServerlessWithTest2") {
			sh "$DOTNET_PATH/dotnet-lambda deploy-serverless ${FUNCTION_NAME}"
		}
	}*/
	
}
