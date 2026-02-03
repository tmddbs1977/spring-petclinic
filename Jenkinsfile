pipeline {
  agent any

  tools {
    jdk "JDK17"
  }

  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    K8S_CREDENTIALS = credentials('kubeconfig')
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
          docker build -t spring-petclinic2:$BUILD_NUMBER .
          docker tag spring-petclinic2:$BUILD_NUMBER tmddbs1977/spring-petclinic2:latest
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
           docker push tmddbs1977/spring-petclinic2:latest
           """
      }
    }

    // Docker Image Remove
    stage('Docker Image Remove') {
      steps {
        echo 'Docker Image Remove'
        sh 'docker rmi -f spring-petclinic2:$BUILD_NUMBER'
      }
    }
    
    // k8s deployment apply
    stage('k8s deployment apply') {
      steps {
        echo 'k8s deployment apply'
        sh 'cd /home/ubuntu/metallb/jenkins'
        withCredentials([file(credentialsId: 'kubeconfig', variable: 'K8S_CREDENTIALS')]) {
            sh '''
                export KUBECONFIG=$K8S_CREDENTIALS
                kubectl apply -f petclinic-deployment1.yaml
                kubectl apply -f petclinic-service1.yaml
                kubectl apply -f petclinic-ingress.yaml
                kubectl get pod -A
                kubectl get deploy -A
            '''
        }
    }
    }
    
  }
}
