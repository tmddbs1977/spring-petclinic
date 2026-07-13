- 프로젝트명

Iac와 하이브리드 클라우드를 활용한 인프라 구축


- 프로젝트 목표

AWS 클라우드와 온프레미스를 VPN으로 연동하여 IaC를 이용한 인프라를 구축하고 Jenkins와 GitHub를 활용한 CI/CD 작업을 수행함으로써 IaC와 하이브리드 클라우드를 이용한 인프라 구축 프로젝트 수행 및 프로젝트 관리 능력을 함양하고자 함.
  

- 사용 기술

· AWS
· Kubernetes
· Docker
· Jenkins
· Ansible


- 내 역할

AWS 클라우드 및 온프레미스 인프라 구축


- 실행 방법

사용자가 깃허브를 업데이트하는것을 트리거로 젠킨스 배포 동작


- 트러블슈팅

문제 상황 :
GitHub 코드 수정 → Jenkins 빌드 → Docker Hub Push까지 성공했으나, 운영 서버(EC2)에서는 계속 이전 버전의 화면이 출력됨.

원인 분석 :
Docker의 레이어 캐싱 latest 태그는 이름일 뿐이며, 로컬에 동일한 이름의 이미지가 있으면 Docker는 외부에서 새로 다운로드(Pull)하지 않고 로컬 이미지를 그대로 사용함.

해결 방법 :
배포 스크립트에 docker compose pull 명령어를 추가하여 배포 시마다 Docker Hub에서 최신 이미지를 강제로 가져오도록 수정.
docker compose up -d --force-recreate 옵션을 통해 이미지 레이어 변화와 상관없이 컨테이너를 강제 재생성함.
