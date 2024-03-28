pipeline{
  agent { label 'Jenkins-Agent' } 
  tools {
    jdk  "Java17"
    maven "maven3"
    
  }
  
  stages {
     stage('1. Cleanup Workspace'){
      steps{
      cleanWs()
      }
    }
    
    stage('2. CloneCode'){
      steps{
        sh "echo 'cloning the latest application version' "
        git branch: 'master', credentialsId: 'Github-Credentials', url: 'https://github.com/Iron-chest/spring-boot-docker'
      }
    }

     stage('3. Test Application'){
      steps{
        sh "mvn test"
      }
    }
    
    stage('4. BuildApplication'){
      steps{
        sh "echo 'creating artifacts from the cloned code' "
        sh "mvn clean package"
      }
    }

   stage('5. SonarQube Analysis'){
      steps{
        script {
            withSonarQubeEnv(credentialsId: 'Jenkins-SonarQube-Token') {
               sh "mvn sonar:sonar"
               }
           }
        } 
     }
    }
 }
