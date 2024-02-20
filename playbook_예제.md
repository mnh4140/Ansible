# 첫번째
``` yaml
- name: set up
  hosts: prod # 인벤토리 호스트
  become_user: root # 실행 계정
  become: yes
  tasks:

    # 유저 생성
   - name: add user
     user:
      name: security_temp
      password: $6$keA0jbN9oNtWT0UE$neES8QAPl2X64ZIEt6VHBxvCxsOuUXL/PXhCV1bTTGkdCED6NZO6A4mx6xAcAnYPI1ESn4yi9PSIjB25XvU.O.

    #sudo 권한 설정
   - name: modify visudo
     lineinfile:
      path: /etc/sudoers
      insertafter: '^root*'
      state: present
      line: "security_temp\tALL=(ALL)\tALL"
      validate: 'visudo -cf %s'
    # sshd 설정 변경
   - name: modify sshd_config
     replace:
      path: /etc/ssh/sshd_config
      regexp: 'PasswordAuthentication no'
      replace: 'PasswordAuthentication yes'
    # sshd 서비스 재시작
   - name: restart sshd
     service:
      name: sshd
      state: restarted

- name: set up
  hosts: dev
  become_user: root
  become: yes
  tasks:

   - name: add user
     user:
      name: security_temp
      password: $6$keA0jbN9oNtWT0UE$neES8QAPl2X64ZIEt6VHBxvCxsOuUXL/PXhCV1bTTGkdCED6NZO6A4mx6xAcAnYPI1ESn4yi9PSIjB25XvU.O.
   - name: modify visudo
     lineinfile:
      path: /etc/sudoers
      insertafter: '^root*'
      state: present
      line: "security_temp\tALL=(ALL)\tALL"
      validate: 'visudo -cf %s'
   - name: modify sshd_config
     replace:
      path: /etc/ssh/sshd_config
      regexp: 'PasswordAuthentication no'
      replace: 'PasswordAuthentication yes'
   - name: restart sshd
     service:
      name: sshd
      state: restarted

- name: set up
  hosts: stg
  become_user: root
  become: yes
  tasks:

   - name: add user
     user:
      name: security_temp
      password: $6$keA0jbN9oNtWT0UE$neES8QAPl2X64ZIEt6VHBxvCxsOuUXL/PXhCV1bTTGkdCED6NZO6A4mx6xAcAnYPI1ESn4yi9PSIjB25XvU.O.
   - name: modify visudo
     lineinfile:
      path: /etc/sudoers
      insertafter: '^root*'
      state: present
      line: "security_temp\tALL=(ALL)\tALL"
      validate: 'visudo -cf %s'
   - name: modify sshd_config
     replace:
      path: /etc/ssh/sshd_config
      regexp: 'PasswordAuthentication no'
      replace: 'PasswordAuthentication yes'
   - name: restart sshd
     service:
      name: sshd
      state: restarted
```

# 두번쨰
```yaml
 tasks:
    # Asia 시간 세팅
    - name: Set OS Time
      timezone:
        name: Asia/Seoul

    # 시간 동기화
    - name: Sync OS Time
      shell: systemctl restart rsyslog
      shell: systemctl restart chronyd

    # Redhat 패키지 설치
    - name: Install epel
      shell: amazon-linux-extras install -y epel
      args:
        executable: /bin/bash  

    # Python3 설치
    - name: Install Python3
      yum:
        state: installed
        name: 
          - python3

    # awscli / boto3
    - name: Install Python lib
      pip:
        executable: pip3
        name:
          - awscli
          - boto3

    # 기타 패키지 설치
    - name: Install software requirements
      yum:
        state: installed
        name: 
          - telnet
```
-----
## Ad-Hoc 명령  
출처 : https://velog.io/@hsshin0602/%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4-130-ansible  
Ansible ad-hoc 명령은 하나 이상의 관리 노드에 단일 작업을 실행하는 임시명령이다.   
임시 명령은 거의 반복하지 않는 간단한 작업에 주로 사용한다.   
(서버 재부팅, 파일 관리, 패키지 관리, 사용자 및 그룹 관리, 서비스 관리, 팩스 변수 수집)  

Ad-hoc 명령 사용
          
    ansible [pattern] -m [module] -a "[module options]"
    -m: 모듈 이름 지정(기본값: command)
    -a: 모듈의 옵션/아규먼트

ping 보내기  

    ansible ansi-node1 -m ping

command

    ansible ansi-node3 -m command -a "/sbin/reboot"
    ansible ansi-node3 -m command -a "/sbin/poweroff"
    ansible ansi-node3 -a "/sbin/poweroff"

시스템 reboot  

    ansible ansi-node3 -m reboot 

파일 복사  

    ansible AA -m copy -a "src=/home/vagrant/ansitest/testfile dest=/home/vagrant/"

※ 소유자가 root로 되어있는데 이는 위에서 ansible.cfg파일에  [privilege_escalation] become=True로 되어있기 때문이다.  
이를 False로 바꾼 뒤 실행해주면 vagrant로 소유자가 바뀌는 것을 확인 할 수 있다.  

파일 권한 변경

    ansible webservers -m file -a "dest=/srv/foo/a.txt mode=600"

파일 소유권 변경

    ansible webservers -m file -a "dest=/path/to/c mode=755 owner=mdehaan group=mdehaan state=directory"

파일 삭제

    ansible webservers -m file -a "dest=/path/to/c state=absent"

패키지 설치(업데이트 X)

    ansible ansi-node3 -m apt -a "name=apache2 update_cache=yes state=preset"
    ansible ansi-node3 -m apt -a "name=apache2 state=present"
    ssh ansi-node3 systemctl status apache2

특정 버전 패키지 설치  

    ansible webservers -m apt -a "name=acme=1:0.96.4-5 state=present"

service 중지, 재시작

    # service 중지
    ansible ansi-node3 -m service -a "name=apache2 state=stopped"
    ssh ansi-node3 systemctl status apache2

    # service 재시작
    ansible ansi-node3 -m service -a "name=apache2 state=started"
    ssh ansi-node2 systemctl status apache2


패키지 제거  

    ansible webservers -m apt -a "name=acme state=absent"

사용자 생성  

    ansible all -m user -a "name=foo password=< crypted password here>"

사용자 제거  

    ansible all -m user -a "name=foo state=absent"


