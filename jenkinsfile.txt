node('master') 
{
    stage('continuous_downlode') 
    {
        git 'https://github.com/peddi130894/ci-cd.git'
    }

    stage('continuous_build')
    {
        sh 'mvn package'
    }
    
   stage('continuous_deployment')
    {
        sh 'scp /home/ubuntu/.jenkins/workspace/pipeline/webapp/target/webapp.war ubuntu@172.31.25.219:/var/lib/tomcat9/webapps/myqa.war'
    }
    
    stage('continuous_testing')
    {
        git 'https://github.com/intelliqittrainings/FunctionalTesting.git'
        sh 'java -jar /home/ubuntu/.jenkins/workspace/pipeline/testing.jar'
    }
    
    stage('continuous_delivey')
    {
        input message: 'waiting for approval from DM', submitter: 'shanker'
        sh 'scp /home/ubuntu/.jenkins/workspace/pipeline/webapp/target/webapp.war ubuntu@172.31.27.190:/var/lib/tomcat9/webapps/myprod.war'
    }
}
