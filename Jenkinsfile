node{

def MavenHome=tool name:"Maven 3.8.6"

echo "GitHub BranhName ${env.BRANCH_NAME}"
      echo "Jenkins Job Number ${env.BUILD_NUMBER}"
      echo "Jenkins Node Name ${env.NODE_NAME}"
  
      echo "Jenkins Home ${env.JENKINS_HOME}"
      echo "Jenkins URL ${env.JENKINS_URL}"
      echo "JOB Name ${env.JOB_NAME}"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '5', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

stage('CheckoutCodeFromGitHub')
{
git credentialsId: '4babf99f-77ae-444a-bd52-5489880493f7', url: 'https://github.com/my-dev-organizatio/maven-web-application-1.git'
}

stage('build')
{
sh "${MavenHome}/bin/mvn clean package"
}

stage('SonarQubeReport')
{
sh "${MavenHome}/bin/mvn sonar:sonar"
}

stage('UploadToNexus')
{
sh "${MavenHome}/bin/mvn clean deploy"
}

stage('DeployToTomcat')
{
sshagent(['4aff5c91-7294-448d-97f3-43499dc17dc1']) {
sh "scp -o StrictHostkeyChecking=no target/maven-web-application.war ec2-user@43.205.115.158:/opt/apache-tomcat-9.0.69/webapps/"  
}
}

stage('EmailNotification')
{
emailext body: '''Hi team,

Amazon build got completed

Thanks & Regards
siraz''', subject: 'pipeline script way', to: 'shaik.sirazz@gmail.com'
}

}
