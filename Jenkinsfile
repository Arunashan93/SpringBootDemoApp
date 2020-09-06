node {
try {
    def server = Artifactory.server 'Artifactory'
    def rtMaven = Artifactory.newMavenBuild()
    def buildInfo
 
  stage('SCM Checkout') { 
  checkout([$class: 'GitSCM', branches: [[name: '*/master']], 
  doGenerateSubmoduleConfigurations: false, 
  extensions: [], submoduleCfg: [], 
  userRemoteConfigs: [[
  url: 'https://github.com/Arunashan93/SpringBootDemoApp.git']]])
  }
  stage('Artifactory Config') {
  rtMaven.tool = 'maven-3'
  rtMaven.deployer releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local', server: server
  buildInfo = Artifactory.newBuildInfo()
  buildInfo.env.capture = true
 }
 stage('Maven-install & Push to Artifactory') {
 rtMaven.run pom:  'pom.xml', goals: 'clean install', buildInfo: buildInfo
}
stage('Email Notification')
 {
    emailext body: '''$PROJECT_NAME - Build # $BUILD_NUMBER :
    Check console output at $BUILD_URL to view the results.''',
    subject:  '$PROJECT_NAME - Build # $BUILD_NUMBER ',
    to: 'arunashanubhogue@gmail.com'
 }
  stage('Publish to Jfrog') {
  server.publishBuildInfo buildInfo
 }
  stage('SonarQube Analysis')
      environment{
          PATH="/usr/share/maven/bin:$PATH"
      }
      withSonarQubeEnv('sonarqube') {
        sh 'mvn sonar:sonar' 
      }   
  stage('Quality Gate Check'){
      sleep(60)
      def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                  currentBuild.result = 'FAILURE'
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }  }}
catch (exception){
  throw exception
}
finally {
    emailext body: '''$PROJECT_NAME - Build # $BUILD_NUMBER :
    Check console output at $BUILD_URL to view the results.''',
    subject:  '$PROJECT_NAME - Build # $BUILD_NUMBER ',
    to: 'arunashanubhogue@gmail.com'
}
}

