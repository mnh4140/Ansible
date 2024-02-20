# 초기 설정 플레이북

## 설정 항목 리스트 (총 9개)
1. 타임존 변경
2. 히스토리 명령어 출력 시 시간 나오게 설정
3. ssh 기본 설정
4. root 계정 wins 계정 패스워드 설정
5. ftp 접속 시 root 제한
6. selinux 설정
7. firewalld 서비스 비활성화
8. 네트워크 툴 설치
9. NTP 설정

### 백업 수행 대상 (9개 증 6개)
- 예외 사유
    - 4번 항목 : root 계정 wins 계정 패스워드 설정
    - 7번 항목 : firewalld 서비스 비활성화
    - 8번 항목 : 네트워크 툴 설치

### 백업 해야 될 파일 목록
1. /etc/localtime (타임존 파일)
2. /etc/profile (히스토리 형식)
3. /etc/ssh/sshd_config (SSH 설정)
4. /etc/ftpusers (ftp 설정 파일)
5. /etc/selinux/config (selinux 설정파일)
6. /etc/chrony.conf (ntp 설정 파일)

```yaml
  - name: "U-01) ssh root permit" # ssh root 로그인 차단
    replace:
      path: /etc/ssh/sshd_config
      regexp: 'PermitRootLogin no'
      replace: 'PermitRootLogin yes'

### STEP8
  - name: "Network Tool Install" # dnf로 패키지 설치
    dnf:
      name: iputils, net-tools, tcpdump
      state: latest
  - name: "설치 확인"
    dnf:
      name: iputils, net-tools, tcpdump
      state: present


  - name: Display the result
    debug:
      msg: "The RPM is {{ 'installed' if rpm_check_result.rc == 0 else 'not installed' }}"


### STEP 9
  - name: "NTP Client Configuration" # NTP 설정
    replace:
      path: /etc/chrony.conf
      regexp: 'pool 2.rocky.pool.ntp.org iburst'
      replace: '#pool 2.rocky.pool.ntp.org iburst'
    lineinfile:
      path: /etc/chrony.conf
      insertafter: '#pool 2.rocky.pool.ntp.org iburst'
      line: "server time.bora.net iburst"
```


```yaml
- name: "Ubuntu 22.04 Initial set"
  hosts: ubuntu
  become: yes
  gather_facts: no

  tasks:
    - name: "0. Config File Backup"
      command: date +"%Y-%m-%d_%H-%M-%S"
      register: ldate

    - name: Backup 디렉토리 생성
      file:
        path: "/root/winsBackup_{{ ldate.stdout }}"
        state: directory

    - name: Backup 파일 복사
      copy:
        src: /etc/localtime
        dest: "/root/winsBackup_{{ ldate.stdout }}/localtime"

    - name: /etc/profile 복사
      copy:
        src: /etc/profile
        dest: "/root/winsBackup_{{ ldate.stdout }}/profile"

    - name: /etc/ssh/sshd_config 복사
      copy:
        src: /etc/ssh/sshd_config
        dest: "/root/winsBackup_{{ ldate.stdout }}/sshd_config"


    #   ubuntu22.04 ftp 미설치
    # - name: /etc/ftpusers 복사
    #   copy:
    #     src: /etc/ftpusers
    #     dest: "/root/winsBackup_{{ ldate.stdout }}/ftpusers"

    #   ubuntu22.04 selinux 미설치
    # - name: /etc/selinux/config 복사
    #   copy:
    #     src: /etc/selinux/config
    #     dest: "/root/winsBackup_{{ ldate.stdout }}/selinux_config"

    #   ubuntu22.04 경로 다름
    - name: /etc/chrony/chrony.conf 복사
      copy:
        # src: /etc/chrony.conf 
        src: /etc/chrony/chrony.conf
        dest: "/root/winsBackup_{{ ldate.stdout }}/chrony.conf"

    - name: "1. UTC를 KST로 변경"
      file:
        src: /usr/share/zoneinfo/Asia/Seoul
        dest: /etc/localtime
        owner: root
        group: root
        state: link


    - name: "3. SSH Password Login"
      replace:
        path: /etc/ssh/sshd_config
        # regexp: 'PermitRootLogin no'
        regexp: '#PermitRootLogin prohibit-password'
        replace: 'PermitRootLogin yes'
```


찐버전
```yaml
#  ##   ##   ####    ##   ##   #####              ####   ####      #####   ##   ##  #####             ##   ##   #####   ######            ######   #######    ##     ##   ## #
#  ##   ##    ##     ###  ##  ##   ##            ##  ##   ##      ##   ##  ##   ##   ## ##            ### ###  ##   ##   ##  ##           # ## #    ##   #   ####    ### ### #
#  ##   ##    ##     #### ##  #                 ##        ##      ##   ##  ##   ##   ##  ##           #######  #         ##  ##             ##      ## #    ##  ##   ####### #
#  ## # ##    ##     ## ####   #####            ##        ##      ##   ##  ##   ##   ##  ##           #######   #####    #####              ##      ####    ##  ##   ####### #
#  #######    ##     ##  ###       ##           ##        ##   #  ##   ##  ##   ##   ##  ##           ## # ##       ##   ##                 ##      ## #    ######   ## # ## #
#  ### ###    ##     ##   ##  ##   ##            ##  ##   ##  ##  ##   ##  ##   ##   ## ##            ##   ##  ##   ##   ##                 ##      ##   #  ##  ##   ##   ## #
#  ##   ##   ####    ##   ##   #####              ####   #######   #####    #####   #####             ##   ##   #####   ####               ####    #######  ##  ##   ##   ## #



- name: "Cloud MSP Initial set"
  hosts: all #ubuntu
  become: yes
  gather_facts: no

  tasks:
     ###############################
     ###### STEP 0. 백업

    - name: "STEP 0. Config File Backup"
      command: date +"%Y-%m-%d_%H-%M-%S"
      register: ldate

    - name: "STEP 0-1. Backup 디렉토리 생성"
      file:
        path: "/root/winsBackup_{{ ldate.stdout }}" /home/ansible/winsBackup_{{ ldate.stdout }}
        state: directory

    - name: "STEP 0-2. /etc/localtime Backup"
      copy:
        src: /etc/localtime
        dest: "/root/winsBackup_{{ ldate.stdout }}/localtime"

    - name: "STEP 0-3. /etc/profile Backup"
      copy:
        src: /etc/profile
        dest: "/root/winsBackup_{{ ldate.stdout }}/profile"

    - name: "STEP 0-4. /etc/ssh/sshd_config Backup"
      copy:
        src: /etc/ssh/sshd_config
        dest: "/root/winsBackup_{{ ldate.stdout }}/sshd_config"

    #   ubuntu22.04 ftp 미설치
    - name: "STEP 0-5. /etc/ftpusers Backup"
      copy:
        src: /etc/ftpusers
        dest: "/root/winsBackup_{{ ldate.stdout }}/ftpusers"

    #   ubuntu22.04 selinux 미설치
    - name: "STEP 0-6. /etc/selinux/config Backup"
      copy:
        src: /etc/selinux/config
        dest: "/root/winsBackup_{{ ldate.stdout }}/selinux_config"

    #   ubuntu22.04 경로 다름
    - name: "STEP 0-7. /etc/chrony/chrony.conf Backup"
      copy:
        src: /etc/chrony.conf
        # src: /etc/chrony/chrony.conf
        dest: "/root/winsBackup_{{ ldate.stdout }}/chrony.conf"

     ###############################


     ###############################
     ###### STEP 8. 네트워크 기본 툴 패키지 설치

    - name: "STEP 8-1. Network Tool Install" # dnf로 패키지 설치
      dnf:
        name: iputils, net-tools, tcpdump
        state: latest # 최신버전이 아니면 업데이트도 진행
        # state: present # 설치 확인만

    - name: "STEP 8-2. Check if the RPM is installed"
      command: "rpm -q iputils,net-tools,tcpdump"
      register: rpm_check_result
      ignore_errors: true  # RPM이 설치되어 있지 않으면 오류를 무시

    - name: "STEP 8-3. Display the result"
      debug:
        msg: "The RPM is {{ 'installed' if rpm_check_result.rc == 0 else 'not installed' }}"

     ###############################


     ###############################
     ###### STEP 9. NTP 설정

    - name: "STEP 9-1. NTP Client Configuration -1" # 기본 ntp 설정 주석
      replace:
        path: /etc/chrony.conf
        regexp: 'pool 2.rocky.pool.ntp.org iburst'
        replace: '#pool 2.rocky.pool.ntp.org iburst'

    - name: "STEP 9-2. NTP Client Configuration -2" # time.bora.net 추가
      lineinfile:
        path: /etc/chrony.conf
        insertafter: '#pool 2.rocky.pool.ntp.org iburst'
        line: "server time.bora.net iburst"

```