pipeline{
  agent any
  tools {
   maven 'maven-9.6.16'
  }

  stages {
    stage('Git Checkout'){
      steps{
       git branch: 'dev', url: 'https://github.com/kkdevops20031988/maven-webapplication-projectkk.git'
      }
    }

    stage('Compile') {
     steps{
        sh 'mvn compile'
     }
    }

    stage('Build') {
     steps{
        sh "mvn clean package"
     }
    }
/*
    stage('SQ Report') {
     steps{
      sh "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:sonar"
     }
    }
*/
    stage('Nexus Deploy') {
     steps {
      sh "mvn clean deploy"
     }
    }

    stage('Deploy to Tomcat') {
     steps {
      sh """

      curl -u mahesh:mahesh123 \
--upload-file /var/lib/jenkins/workspace/Elig_Declarative_PL_Dev/target/maven-web-application.war \
"http://3.110.45.16:8080/manager/text/deploy?path=/maven-web-application&update=true"
          
        """
     }
    }
  } //stages ending

 post {
  success{
    script {
     notifyBuild(currentBuild.result)
    }
  }
  failure{
    script {
     notifyBuild(currentBuild.result)
    }
  }
 }
    
} // pipeline ending

// Notification method
def notifyBuild(String buildStatus = 'STARTED') {
    buildStatus = buildStatus ?: 'SUCCESS'

    def colorCode
    def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
    def summary = "${subject} (${env.BUILD_URL})"

    switch (buildStatus) {
        case 'STARTED':
            colorCode = '#FFFF00' // Yellow
            break
        case 'SUCCESS':
            colorCode = '#00FF00' // Green
            break
        default:
            colorCode = '#FF0000' // Red
    }

    slackSend(color: colorCode, message: summary, channel: '#jio-om')
}
