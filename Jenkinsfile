pipeline {
  agent any

  tools {
    jdk "JDK17"
  }

  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
  }

  stages {
    // Git Clone
    stage('Git Clone') {
      steps {
        git url: 'https://github.com/tmddbs1977/spring-petclinic.git', branch: 'main'
      }
    }
    //Maven을 이용해 Build 한다.
    stage('Maven Build') {
      steps {
        sh 'chmod +x mvnw'
        sh './mvnw clean package -DskipTests'
      }
      post {
        success {
          echo 'Maven Build Sucess'
        }
        failure {
          echo 'Maven Build Failed'
        }
      }
    }
    
    // Docker Image 생성
    stage('Docker Image Build') {
      steps {
        echo 'Docker Image Build'
        dir("${env.WORKSPACE}"){
          sh """
          docker build -t spring-petclinictest:$BUILD_NUMBER .
          docker tag spring-petclinictest:$BUILD_NUMBER tmddbs1977/spring-petclinictest:latest
          """
        }
      }
    }
    
    // Docker Image Upload
    stage('Docker Image Upload') {
      steps {
        echo 'Docker Image Upload'
        sh """
           echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
           docker push tmddbs1977/spring-petclinictest:latest
           """
      }
    }

    // Docker Image Remove
    stage('Docker Image Remove') {
      steps {
        echo 'Docker Image Remove'
        sh 'docker rmi -f spring-petclinictest:$BUILD_NUMBER'
      }
    }
    
  }
}
