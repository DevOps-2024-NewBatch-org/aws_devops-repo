node

{

// its a variable name (/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/maven-3.9.5)

def mavenHome = tool name: "maven3.8.5"	

// enable poll scm and discard builds using job properties

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])

// using env variables do this activities

echo "the job name is: ${env.JOB_NAME}"

echo "the node name is: ${env.NODE_NAME}"

echo "the build no is: ${env.BUILD_NUMBER}"
	
	
// download the code from github
	
stage('get the code from Github'){
git credentialsId: '62c64a77-bb63-429f-aa2d-b907f267fa38', url: 'https://github.com/DevOps-2024-NewBatch/aws_devops.git'
}

// build the package using maven goal

stage('Build the package'){
sh "${mavenHome}/bin/mvn clean package"		
}

// send the report to sonarQube server

stage('Generate sonarQube report'){
sh "${mavenHome}/bin/mvn sonar:sonar"
}

// send the artifact id to nexus server

stage('Generate artifact ID and send to nexus'){
sh "${mavenHome}/bin/mvn deploy"
}

// deploy app in to tomcat server and ssh agent plugin dwara ssh key ni configure chesamu

stage('deploy app in to tomacat server'){
sshagent(['2aefce28-0f69-4469-be4d-d3df7be8bcd8']) {
    sh "scp -o StrictHostKeyChecking=no target/web-app.war ec2-user@172.31.4.5:/opt/tomacat-9.2/webapps/" 
}
}

}
