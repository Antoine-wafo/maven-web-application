pipeline{
  agent any 
  tools {
    maven "maven3.8.7"
  }  
  stages {
    stage('1GetCode'){
      steps{
        sh "echo 'cloning the latest application version' "
        git branch: 'feature', credentialsId: 'githubcredentials', url: 'https://github.com/Antoine-wafo/maven-web-application.git'
      }
    }
    stage('3Test+Build'){
      steps{
        sh "echo 'running JUnit-test-cases' "
        sh "echo 'testing must passed to create artifacts ' "
        sh "mvn clean package"
      }
    }
    stage('4CodeQuality'){
      steps{
        sh "echo 'Perfoming CodeQualityAnalysis' "
        sh "mvn sonar:sonar"
  }
    } 
    stage('5uploadNexus'){
      steps{
        sh "mvn deploy"
      }
    }  
    stage('8deploy2prod'){
      steps{
        deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://44.210.69.15:8080/')], contextPath: null, war: 'target/*war'
   
      }
     }
  }
    post{
    always{
      emailext body: '''Hey guys
Please check build status.

Thanks
Landmark 
+1 443 531 6471''', recipientProviders: [buildUser(), developers()], subject: 'success', to: 'wafoantoine@yahoo.fr' 
      }
   }
  }
