# Spring PetClinic - CI/CD Application Repository

AWS IaC 및 CI/CD 프로젝트에서 배포 테스트용 애플리케이션으로 사용한 Spring PetClinic 기반 저장소입니다.

원본 Spring PetClinic 애플리케이션에 프로젝트에서 사용한 CI/CD 및 배포 관련 파일을 추가하여 Jenkins, Docker Hub, AWS CodeDeploy 배포 테스트에 활용했습니다.

## 프로젝트에서 사용한 주요 파일

- `Jenkinsfile` : Jenkins CI/CD Pipeline
- `Dockerfile` : Spring PetClinic Docker Image 생성
- `appspec.yml` : AWS CodeDeploy 배포 정의
- `scripts/` : Docker Compose 및 배포 실행 스크립트

## 관련 포트폴리오

AWS 인프라 및 CI/CD 구성에 대한 상세 내용은 아래 포트폴리오 저장소에 정리했습니다.

https://github.com/tmddbs1977/aws-iac-cicd-project
