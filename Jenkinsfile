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

    stage('SQ Report') {
     steps{
      sh "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:sonar"
     }
    }

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
"http://15.206.174.234:8080/manager/text/deploy?path=/maven-web-application&update=true"
          
        """
     }
    }

    stage('bsnl-qa') {
     steps{
       build job: 'BSNL-QA' //this is downstream of Dev
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
