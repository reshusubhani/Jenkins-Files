node
{
  def mavenHome = tool name: "maven3.8.1" 
  properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
stage('Git-Checkout')
   {
   git branch: 'dev', credentialsId: '713c61b7-a94a-459a-8a5b-763567813e07', url: 'https://github.com/reshusubhani/javaproject.git'
   }
  stage('Maven-build')
   {
   sh "${mavenHome}/bin/mvn clean package"
   }
    stage('ExecutrSonarQubeReport')
   {
  sh "${mavenHome}/bin/mvn sonar:sonar"
   }
stage('UploadArtifactsIntoNexus')
   {
  sh "${mavenHome}/bin/mvn deploy"
   }
   stage('DeployAppIntoTomcat')
  {
   sshagent(['a4472a1d-2c0e-48bf-82b6-41b50307f0e2'])
     {
      sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@54.90.224.101://opt/apache-tomcat-9.0.45/webapps"
     }
  }
  stage('EmailNotification')
{
emailext body: '''Build is over

Regards,
Subhani
#9848345618''', subject: 'Build is over', to: 'subhanireshu@gmail.com'
}
}
