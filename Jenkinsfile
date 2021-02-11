node
{
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), parameters([choice(choices: ['master', 'development'], description: 'branchName uses for selecting branch', name: 'branchName')]), pipelineTriggers([pollSCM('* * * * *')])])
    def mavenHome = tool name: "maven3.6.3"
    stage("codeCheckOut")
    {
        git branch: 'development', credentialsId: 'e321cad8-6346-4ad1-ade4-5be9c6eaa632', url: 'https://github.com/dilip-ec/maven-web-application-master.git'
    }
    stage("Buid")
    {
        sh "${mavenHome}/bin/mvn clean package"    
    }
    stage("sonarQubeReport")
    {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage("artifactUpload")
    {
        sh "${mavenHome}/bin/mvn deploy"
    }
    stage("deployApplicationIntoTomcat")
    {
        sshagent(['0c78479e-4c74-4edd-a656-13e21de538b7'])
        {
            sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.232.159.70:/opt/apache-tomcat-9.0.41/webapps/"
         }
    }
    stage("sendEmailNotification")
    {
        emailext body: '''BuilisOver

Regards 

Dilipkumar''', subject: 'BuildisOver', to: 'dilipkumar0396@gmail.com'
    }
}
