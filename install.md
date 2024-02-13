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
   4. 다시 ansible 설치 명령어 수행
   5. dnf install ansible -y
   6. 설치 확인
   - ansible --version
