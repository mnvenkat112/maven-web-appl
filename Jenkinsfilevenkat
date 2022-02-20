
node
{
  def mavenHome = tool name: "maven 3.8.4"

  stage('checkoutcode')
  {
  git branch: 'development', credentialsId: '23e7fc39-cec9-4bce-b8c1-0d7965f078d0', 
  url: 'https://github.com/mnvenkat112/maven-web-appl.git'
  }

  stage('build')
  {
   sh "${mavenHome}/bin/mvn clean package"

  }
 
  stage('excutesonarqube')
  {
  sh "${mavenHome}/bin/mvn clean sonar:sonar"
  }
  
  stage('uploadpackagenexus')
  {
  sh "${mavenHome}/bin/mvn clean deploy"
  }

  stage('deployappintoTomcat')
  {
 sshagent(['9073ac45-cb89-42e2-8e55-8a18af8a19db']) 
  {
     sh "scp -o  StrictHostKeyChecking=no  target/maven-web-application.war  ec2-user@3.110.37.181:/opt/apache-tomcat-9.0.56/webapps/"
  }
  }

  stage('sendemailnotification')
  {
   
  emailext body: '''build over...


  regards,
  mnvenkat.''', subject: 'build over', to: 'mnvenkat112@gmail.com'



  }
}
