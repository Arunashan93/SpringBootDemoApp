node{
stage('SCM Checkout'){
git 'https://github.com/Arunashan93/SpringBootDemoApp.git'
}
stage('Compile-test-Package'){
def mvnHome = tool name: 'maven-3',type: 'maven'
sh "${mvnHome}/bin/mvn package"
}
}
