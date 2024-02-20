### 1. 환경구성
   - rocky8 리눅스 2대
### 2. git 설치
   - dnf install -y git
   - 마스터 서버에 설치
   - git 클론
   - ![image](https://github.com/mnh4140/Ansible/assets/71053769/f44eb02b-03de-4c73-b5fc-aa952aaec083)

### 3. ansible 설치
 1. 설치 전 작업 / 레파지토리 추가
    - dnf install ansible -y
    - 기본 리파지토리에 ansible 패키지를 제공하지 않아 아래의 오류 발생
   ![image](https://github.com/mnh4140/Ansible/assets/71053769/96af3de3-2d62-4348-b544-b6bad4758395)
    - EPEL 레파지토리에서 설치하면 ansible 사용가능
      <pre>dnf install epel-release -y</pre>
      ![image](https://github.com/mnh4140/Ansible/assets/71053769/3ec372b1-a8f1-4937-964b-c50de5d80c45)
      
 2. 다시 ansible 설치 명령어 수행
    <pre>dnf install ansible -y</pre>   
    <pre>
    [root@ip-10-0-14-27 Ansible]# dnf install ansible -y
    Extra Packages for Enterprise Linux 8 - x86_64                     11 MB/s |  16 MB     00:01
    Last metadata expiration check: 0:00:05 ago on Tue 13 Feb 2024 01:42:33 AM UTC.
    Dependencies resolved.
    ==================================================================================================
    Package                            Architecture  Version                  Repository        Size
    ==================================================================================================
    Installing:
    ansible                            noarch        8.3.0-1.el8              epel              41 M
    Installing dependencies:
    ansible-core                       x86_64        2.15.3-1.el8             appstream        3.6 M
    mpdecimal                          x86_64        2.5.1-3.el8              appstream         92 k
    python3.11                         x86_64        3.11.5-1.el8_9           appstream         29 k
    python3.11-cffi                    x86_64        1.15.1-1.el8             appstream        292 k
    python3.11-cryptography            x86_64        37.0.2-5.el8             appstream        1.1 M
    python3.11-libs                    x86_64        3.11.5-1.el8_9           appstream         10 M
    python3.11-pip-wheel               noarch        22.3.1-4.el8             appstream        1.4 M
    python3.11-ply                     noarch        3.11-1.el8               appstream        134 k
    python3.11-pycparser               noarch        2.20-1.el8               appstream        146 k
    python3.11-pyyaml                  x86_64        6.0-1.el8                appstream        213 k
    python3.11-setuptools-wheel        noarch        65.5.1-2.el8             appstream        719 k
    sshpass                            x86_64        1.09-4.el8               appstream         29 k
    Installing weak dependencies:
    python3-jmespath                   noarch        0.9.0-11.el8             appstream         44 k
    
    Transaction Summary
    ==================================================================================================
    Install  14 Packages

    Total download size: 59 M
      Installed size: 503 M
      </pre>
      ![image](https://github.com/mnh4140/Ansible/assets/71053769/bfbda41d-770d-437f-b11d-91095ae437e4)

   1. 설치 확인
      - ansible --version
      ![image](https://github.com/mnh4140/Ansible/assets/71053769/12739193-076e-401f-8d6c-c66ab9bc078d)

### 4. AWS 환경에서 Ansible 설정
- AWS EC2 접근 위해 PEM File 사용해야됨
- ssh-keygen 후 public key 배포해야는데 ssh-copy-id 사용 불가
- 아래의 명령어 사용 시 가능  
`cat ~/.ssh/id_rsa.pub | ssh -i "KEY_NAME.pem" root@10.0.0.1 "cat - >> ~/.ssh/authorized_keys"`

cat ~/.ssh/id_rsa.pub | ssh -i "NOHUN-KEY.pem" rocky@3.39.187.94 "cat - >> ~/.ssh/authorized_keys"


## Ubuntu 22.04 rpms

Get:1 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu jammy/main amd64 python3-packaging all 21.3-1 [30.7 kB]
Get:2 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu jammy/universe amd64 python3-resolvelib all 0.8.1-1 [23.6 kB]
Get:3 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu jammy/main amd64 python3-jmespath all 0.10.0-1 [21.7 kB]
Get:4 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu jammy/universe amd64 python3-kerberos amd64 1.1.14-3.1build5 [23.0 kB]
Get:5 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu jammy/main amd64 python3-nacl amd64 1.5.0-2 [63.1 kB]
Get:6 https://ppa.launchpadcontent.net/ansible/ansible/ubuntu jammy/main amd64 ansible-core all 2.15.9-1ppa~jammy [1027 kB]
Get:7 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu jammy/universe amd64 python3-ntlm-auth all 1.4.0-1 [20.4 kB]
Get:8 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu jammy-updates/main amd64 python3-paramiko all 2.9.3-0ubuntu1.2 [134 kB]
Get:9 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu jammy/universe amd64 python3-requests-kerberos all 0.12.0-2 [11.9 kB]
Get:10 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu jammy/universe amd64 python3-requests-ntlm all 1.1.0-1.1 [6160 B]
Get:11 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu jammy/universe amd64 python3-xmltodict all 0.12.0-2 [12.6 kB]
Get:12 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu jammy/universe amd64 python3-winrm all 0.3.0-2 [21.7 kB]
Get:13 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu jammy/universe amd64 sshpass amd64 1.09-1 [11.7 kB]
Get:14 https://ppa.launchpadcontent.net/ansible/ansible/ubuntu jammy/main amd64 ansible all 8.7.0-1ppa~jammy [16.5 MB]

Setting up python3-ntlm-auth (1.4.0-1) ...
Setting up python3-resolvelib (0.8.1-1) ...
Setting up python3-kerberos (1.1.14-3.1build5) ...
Setting up sshpass (1.09-1) ...
Setting up python3-xmltodict (0.12.0-2) ...
Setting up python3-packaging (21.3-1) ...
Setting up python3-jmespath (0.10.0-1) ...
Setting up python3-requests-kerberos (0.12.0-2) ...
Setting up python3-nacl (1.5.0-2) ...
Setting up python3-requests-ntlm (1.1.0-1.1) ...
Setting up ansible-core (2.15.9-1ppa~jammy) ...
Setting up python3-winrm (0.3.0-2) ...
Setting up ansible (8.7.0-1ppa~jammy) ...
Setting up python3-paramiko (2.9.3-0ubuntu1.2) ...


Get:1 jammy/universe amd64 python3-ntlm-auth all 1.4.0-1
Get:2 jammy/universe amd64 python3-resolvelib all 0.8.1-1
Get:3 jammy/universe amd64 python3-kerberos amd64 1.1.14-3.1build5
Get:4 jammy/universe amd64 sshpass amd64 1.09-1
Get:5 jammy/universe amd64 python3-xmltodict all 0.12.0-2
Get:6 jammy/main amd64 python3-packaging all 21.3-1
Get:7 jammy/main amd64 python3-jmespath all 0.10.0-1
Get:8 jammy/universe amd64 python3-requests-kerberos all 0.12.0-2
Get:9 jammy/main amd64 python3-nacl amd64 1.5.0-2
Get:10 jammy/universe amd64 python3-requests-ntlm all 1.1.0-1.1
Get:11 jammy/main amd64 ansible-core all 2.15.9-1ppa~jammy
Get:12 jammy/universe amd64 python3-winrm all 0.3.0-2
Get:13 jammy/main amd64 ansible all 8.7.0-1ppa~jammy
Get:14 jammy-updates/main amd64 python3-paramiko all 2.9.3-0ubuntu1.2
