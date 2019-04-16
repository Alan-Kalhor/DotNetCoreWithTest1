def bucket = 'deployment-packages-test1'
def functionName = 'Fibonacci'
def region = 'ap-southeast-2'

node {
    stage('Checkout'){
        checkout scm
    }
	
	stage('Restore Packages') {
		dotnet restore
	}
}