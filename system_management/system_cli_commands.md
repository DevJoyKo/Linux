### System management command lines
<hr>

* System management
  

    - rsyslog: 환경설정 파일) /etc/rsyslog.conf

  
* 로그 관련 파일 
  

    - /var/log/messages: 시스템 표준 메시지. root만 읽을 수 있음.
    - /var/log/secure: 인증 기반 접속 로그 (telnet, tcp wrappers..)
    - /var/log/dmesg: 시스템 부팅시 출력되었던 로그 기록. 커널 부트 메시지 로그
    - /var/log/mailog: sendmail, dovecat 메일 관련 작업 기록
    - /var/log/xferlog: FTP 접속관련 작업 기록
    - /var/log/cron: 
    - /var/log/boot.log: 부팅시 발생 메시지 
    - /var/log/lastlog: telnet, tfp 등 각 사용자의 마지막 정보 기록
    - /var/log/wtmp: 재부팅한 기록, 사용자 기록 등
    - /var/log/btmp: 접속이 실패한 경우


    last: 사용자의 로그인 정보, 재부팅 정보 -> /var/log/wtmp (바이너리 파일로)
    last -f 파일명: 해당 파일 기록보기
    last -n 숫자: n만큼 출력
    last -R: IP주소나 호스트명을 출력 X
    last -a: 호스트명이나 IP주소 필드를 맨 마지막에 출력

    ex) last: /var/log/wtmp가 만들어진 후 관련 정보 출력
    ex) last samadal: samadal 사용자의 로그인 정보 출력
    ex) last reboot: 시스템 재부팅 내역 출력

    lastlog: 각 사용자가 마지막으로 로그인 한 정보 출력 -> /var/log/lastlog
    ex) lastlog -u 사용자
    ex) lastlog -t {지정날짜} 만큼 거슬러 올라가 그 이후에 로그인한 사용자의 정보 알려줌

    lastb: 로그인 실패 기록
    dmesg: 커널 버퍼의 내용을 출력하고 제어


** 시스템 보안 관리

    1. 시스템 보안 관리 

    sysctl 
    -a, -A: 커널 매개변수와 값 모두 출력
    -p: 환경변수 파일에 설정된 값 출력. 파일 지정 안하면 -> /etc/sysctl.conf 
    -n: 특정 매개변수에 대한 값 출력시
    -w 변수=값: 설정
    
    ex) sysctl -n net.ipv4.icmp._echo_ignore_all
    ex) sysctl -w net.ipv4.icmp._echo_ignore_all=0

    
    ssh
    파일) /etc/ssh/sshd_config
    

  