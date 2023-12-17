node{

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

echo "The Job name is: ${env.JOB_NAME}"
echo "The Node name is: ${env.NODE_NAME}"
echo "The Build Number is: ${env.BUILD_NUMBER}"
echo "The Jenkins Hom directory is: ${JENKINS_HOME}"


def mavenHome = tool name: "maven3.9.6"

stage('CheckoutCode'){
git branch: 'development', credentialsId: '84f6d941-e038-44ed-80d9-8a4f3bede4dd', url: 'https://github.com/bankingapps1/maven-web-application.git'
}

stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadArtifactsIntoNexus'){
sh "${mavenHome}/bin/mvn deploy"
}

stage('DeployintoTomcatServer'){
sshagent(['787a60d8-d197-4138-8873-a977c705fe3f']) {
 sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.46.33:/opt/apache-tomcat-9.0.83/webapps/"
 }
}

}
