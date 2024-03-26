pipeline{
  agent { label 'Jenkins-Agent' } 
  tools {
    jdk  "Java17"
    maven "maven3"
  }  
    environment {
          APP_NAME = "spring-boot-app"
          RELEASE = "1.0.0"
          DOCKER_USER = "devopsforjesus"
          DOCKER_PASS = 'Ironchest567'
          IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
          IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
          JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")
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

     // stage('6. Quality Gate'){
     //  steps{
     //    script {
     //        waitForQualityGate abortPipeline: false, credentialsId: 'Jenkins-SonarQube-Token'
     //           }
     //       }
     //   }

        stage('7. Build & Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }
        }
    
    }
 }
