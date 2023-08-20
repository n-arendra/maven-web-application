node {
    
    def mavenHome = tool name: "mavan3.9.4"
    
    echo "Jenkins url is: ${env.JENKINS_URL}"
    echo "Node Name is: ${env.NODE_NAME}"
    echo "Job Name is: ${env.JOB_NAME}"
    
    stage('Checkout code') 
    {
        git branch: 'development', url: 'https://github.com/n-arendra/maven-web-application.git'
    }
    
    stage ('Build') 
    {
        sh "${mavenHome}/bin/mvn clean package"
    }
     stage ('SonarQube Report') 
    {
        sh "${mavenHome}/bin/mvn clean sonar:sonar"
    }
     stage ('Upload artifact in Nexus') 
    {
        sh "${mavenHome}/bin/mvn clean deploy"
    }
    stage ('Deploy app into Tomcat Server') 
    {
        sshagent(['35875b5b-17ef-4d48-8adc-4af19392bb43']) {
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ubuntu@3.138.116.92:/opt/apache-tomcat-9.0.79/webapps"
    }
    }
    
    stage('Send Email')
    {
        emailext body: '''Web application is deployed successfully on tomcat server.

        Thanks and regards
        Narendra chavan''', subject: 'Build successfull..!', to: 'narendrachauhan2026@gmail.com'
    }
}
