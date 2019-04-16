def bucket = 'deployment-packages-test1'
def functionName = 'Fibonacci'
def region = 'ap-southeast-2'

pipeline {

	agent any
	
	stage('Checkout'){
        checkout scm
    }
	
	/*stage('Restore Packages') {
		steps {
			bat "dotnet restore"
		}
	}*/
}