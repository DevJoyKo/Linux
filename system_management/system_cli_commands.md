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



  