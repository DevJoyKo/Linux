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


** 시스템 백업

    Backup 및 Restore
    
    tar: 파일이나 디렉터리 하나로 묶어주는 역할
    - p: 모든 퍼미션 정보 유지

    증분 백업
    tar -g list -cvfp home1.tar /home: g는 증분백업에 사용하는 옵션으로 list 내용을 토대로 증분 백업 시도
    tar xvf home1.tar -C /: 백업한 파일 복원

    cpio: tar와 비슷, 많은 양의 데이터에 대해서 tar에 비해 빠름. 증분백업 x
    cpio (옵션) > (파일명) -> 백업
    cpio (옵션) < (파일명) -> 복원

    -o: 표준 출력으로 보내어 사용
    -i: 표준 입력으로 받을때 사용
    -v: 자세히 출력
    -c: 데이터 형식 ASCII
    -d: 필요한 경우 디렉터리 생성
    -t: 내용만 확인할 때 사용
    -F: 표준 입출력 전환기호 대신에 파일명 지정할 때 사용
    -B: 블록사이즈 조절할 때 사용

    dump: 파일이 아닌 파일 시스템 전체 백업시 사용하는 유틸. 파티션 단위로 백업할 때 많이 사용
    
    dump (옵션) (파일명) (파일대상)
    -0-9: 레벨 지정
    -f: 백업할 매체나 파일명 적음
    -u: 덤프 작업 후에 /etc/dumpdates라는 파일에 관련 정보 기록

    dump -0u -f backup.dump /dev/sdb7: 
    -> /dev/sdb7을 backup.dump 에 전체 백업하고, 작업 정보를 /etc/dumpdates에 기록

    restore (옵션) (백업) (파일명)
    -i: 대화식으로 선택 복원
    -f: 백업할 매체나 파일명
    -r: 전체 복원시 사용. 이 옵션 사용시 파일 시스템이 미리 생성되어 있어야 하고, 마운트도 되어 있어야 함. 
    restore -rf backup.dump: backup.dump에 백업된 데이터 전체 복원

    dd: 파티션이나 디스크 단위로 백업할 때 사용하는 유틸리티
    dd if=dev/sda1 of=/dev/sdb1 bs=1K
    dd if=dev/sda1 of=/dev/sdb1 bs=1M

    rsync: 네트워크로 연결된 원격지의 파일들 동기화 하는 유틸리티
    rsync (옵션) (대상) (목적지)
    -r: 하위 디렉터리 까지
    -ㅣ: 심볼릭 링크를 그대로 보존
    -p: 퍼미션 보존
    -t: 타임스탬프 보존 
    -g: 그룹소유권 보존
    -o: 소유권 보존
    -D: 디바이스 파일 그대로 보존 
    -H: 하드링크 그대로 보존
    -a: -rlptgoD 한번에 실행
    -u: 업데이트 내용만 전송
    -b: 백업할 때 동일한 파일 존재하는 경우 ~를 붙여서 백업 파일 생성
    -z: 전송할때 압축
    -v: 자세히 출력
    --delete: 원본 삭제시 백업도 삭제 

    ex) rsync -av /home /home5: /home 그대로 보존하면서 /home5로 백업
    ex) rsync -avz 192.168.30.128:/home/: 원격지인 192.168.30.128의 /home 압축해서 복사


    
    
    

  