1. 환경구성
   - rocky8 리눅스 2대
2. git 설치
   - dnf install -y git
   - 마스터 서버에 설치
   - git 클론
   - ![image](https://github.com/mnh4140/Ansible/assets/71053769/f44eb02b-03de-4c73-b5fc-aa952aaec083)

3. ansible 설치
   1. 설치 전 작업 / 레파지토리 추가
   - dnf install ansible -y
   - 기본 리파지토리에 ansible 패키지를 제공하지 않아 아래의 오류 발생
   - ![image](https://github.com/mnh4140/Ansible/assets/71053769/96af3de3-2d62-4348-b544-b6bad4758395)
   2. EPEL 레파지토리에서 설치하면 ansible 사용가능
   3. dnf install epel-release -y
   4. ![image](https://github.com/mnh4140/Ansible/assets/71053769/3ec372b1-a8f1-4937-964b-c50de5d80c45)

   5. 다시 ansible 설치 명령어 수행
   6. dnf install ansible -y
   7. ![image](https://github.com/mnh4140/Ansible/assets/71053769/bfbda41d-770d-437f-b11d-91095ae437e4)

   8. 설치 확인
   - ansible --version
   - ![image](https://github.com/mnh4140/Ansible/assets/71053769/12739193-076e-401f-8d6c-c66ab9bc078d)

