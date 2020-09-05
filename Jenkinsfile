node{
stage('SCM Checkout'){
git 'https://github.com/Arunashan93/SpringBootDemoApp.git'
}
stage('Compile-test-Package'){
def mvnHome = tool name: 'maven-3',type: 'maven'
sh "${mvnHome}/bin/mvn package"
}
  stage('Email Notification'){
    mail bcc: '', body: '''$PROJECT_NAME - Build # $BUILD_NUMBER : Check console output at $BUILD_URL to view the results.
''', cc: '', from: '', replyTo: '', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER', to: 'arunashanubhogue@gmail.com'
  }
}
