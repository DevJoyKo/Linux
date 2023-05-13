### network management command lines
<hr>

#### network install and management

- 네트워크 서비스 


    아파치 환경설정: httpd.conf
    /usr/local/apache/conf/httpd.conf
    
    httpd.conf 주요 옵션
    ServerRoot: 웹서버 설치 디렉토리
    Listen: 아파치 웹서버 포트
    User: 실행되는 아파치 데몬 사용자
    Group: 실행되는 아파치 데몬 그룹
    ServerAdmin: 서버 문제가 생겼을 때 보낼 관리자 이메일 주소
    ServerName: 서버 도메인 이름
    DocumentRoot: 웹문서 위치 디렉토리
    DirectoryIndex: 웹 문서중에 처음으로 인식하는 인덱스 문서 지정

    
    아파치 웹 접근 통제
    Order Deny,Allow
    Deny from 주소
    Allow from 주소
    -> Deny 지시자가 Allow지시자 보다 먼저 검사. 대부분 접근을 막고, allow 지시자 지정 주소만 허가

    Order Allow,Deny
    Allow from 주소 
    Deny from 주소
    -> 대부분을 접근 허가하고, Deny 지시자에 지정한 사용자만 접속을 거부할 때

    All: 모든 클라이언트
    특정 도메인명 혹은 해당 도메인 전체 
    ex) samadal.com, .samadal.com

    IP주소
    ex) 192.168.30.128
    
    네트워크 대역
    ex) 192.168.30 / 192.168.30, 192.168.30.0/24


* 인증관련 서비스


NIS: 네트워크 기반 정보 제공
-> 하나의 서버에 등록된 사용자 계정, 암호, 그룹정보 등을 공유해서 다른 시스템에 제공하는 서비스

NIS 이용하기
-> RPC를 사용하기 때문에 관련 데몬을 반드시 구동해야 함. 미사용시, /etc/hosts에 등록
1) rpc 관련 데몬 실행
/etc/rc.d/init.d/rpcbind start
service rpcbind start

2) /etc/hosts 파일 수정
vi /etc/hosts
192.168.30.128 nis.samadal.com


####NIS 서버구성 - NIS서버용 패키지명 'ypserv'
: ypserv 설치시, /etc/rc.d/init.d/안에 ypserv, yppasswd, ypxfrd NIS관련 데몬 스크립트 생성됨

1. 도메인 설정  
nisdomainname 사용하거나, /etc/sysconfig/network에 등록
1) nisdomainname samada.com
2) vi /etc/sysconfig/network -> NISDOMAIN=samada.com

2. 계정생성

3. 데몬실행
service ypserv start
service yppasswdd start
service ypxfrd start

4. 관련 정보 갱신 
cd /var/yp : make
make -C /var/yp

<br>

####NIS 클라이언트 구성 - ypbind, yp-tools 패키지 설치

1. 도메인 설정  
    nisdomainname 사용하거나, /etc/sysconfig/network에 등록
    1) nisdomainname samada.com
    2) vi /etc/sysconfig/network -> NISDOMAIN=samada.com

2. NIS 서버와 nisdomain 설정: /etc.ypconf 파일에 nis서버 및 도메인 설정 
    1) server nis.samadal.com 
    2) ypserv nis.samadal.com 
    3) domain samadal.com

3. 데몬실행
    service ypbind start


    * 관련 명령어
    nisdomainname
    ypwhich
    ypcat
    yptest
    yppasswdd
    ypchsh
    ypchfn
    

#### 파일 관련 서비스

    SAMBA서버관리
    /etc/samba/smb.conf
    
    - Global Setting -> /etc/samba/smb.conf
    comment
    path
    read only
    writable
    valid users
    write list
    public
    browseable
    printable
    create mask 
    
    smbclient (옵션) (호스트명)
    -L: 공유디렉터리 정보 출력
    -U: 삼바 서버 접속할 때 사용자명 입력 
    ex) smbclient -L 192.168.30.128 : 192.168.30.128에 공유된 목록 확인
    ex) smbclient -L joon -U administrator: joon이라는 호스트에 administrator계정으로 접근하여 공유 목록 확인
    ex) smbclient \\\\joon\\source -U administrator
    ex) smbclinet //192.168.30.128/source

    
    NFS 서버관리 - 다른 컴퓨터의 파일 시스템 마운트 하고 공유하여 상대방 파일 시스템 일부를 자신의 것처럼 사용
    1. NFS 서버 설정 - /etc/exports에서 하고, /etc/rc.d/init.d에 존재하는 rpcbind, nfs 실행하여 사용 가능

    - 관련 데몬 시작  
    service rpcbind start
    service nfs start

    ro: 읽기 전용
    rw: 읽기, 쓰기
    root_squash
    no_root_squash
    no_subtree_check
    all_squash
    secure
    async
    anonuid
    anongid
    
    2. NFS 클라이언트 설정 - mount 명령 이용. /etc/fstab에 등록
    mount -t nfs 192.168.30.128:/nfs /mnt
    mount.nfs 192.168.30.128:/nfs1 /mnt 

    /etc/fstab에 설정 내용 
    192.168.30.128:/nfs     /mnt    nfs     timeo=15,soft,retrans=3     0   0

    timeo
    retrans
    soft
    hard
    rsize
    wsize
    fg
    bg

    rpcinfo -> rpc 관련 정보 출력해주는 옵션
    rpcinfo (옵션) (호스트명)
    
    -p: rpc 프로그램의 정보 출력
    -s: 간결하게 출력

    rpcinfo -s 192.168.30.128 
    
    exportfs: nfs 서버에 익스포트 된 디렉터리 정보를 관리해주는 명령
    exportfs (옵션) (호스트명)
    -v:
    -r:
    -a: 
    -u:
    exportfs -au: 익스포트된 모든 디렉터리 해제

    FTP 서버관리
    설정파일 /etc/vsftpd/vsftpd/conf
    anonymous_enable=YSE
    local_enable=YES
    write_enable=YES
    local_umask=022
    anon_upload_enable=YES
    anon_mkdir_write_enable=YES
    dirmessage_enable=YES
    xferlog_enable=YES
    connect_from_port_20=YES
    chown_uploads=YES
    xferlog_file=/var/log/xferlog
    idle_session_timeout=600
    data_connection_timeout=120
    chroot_local_user=YES
    tcp_wrappers=YES
    max_clients=50
    max_per_ip=3

##### 메일관련 서비스

    - /etc/mail/local-host-names
    - /etc/mail/sendmail.mc
    ex) makemap hash /etc/mail/access < /etc/mail/access

    /etc/aliases
    newaliases
    sendmail -bi 

    /etc/mail/virtusertable
    ex) makemap hash /etc/mail/virtusertable < /etc/mail/virtusertable

##### DNS 관리

    /etc/named.conf 
    /var/named zone 파일 위치


    options 구문
    directory
    dump-file
    statistics-file 
    memstatistics-file
    forward only; -> first - 다른 서버에서 응답 없을 때 자신이 응답하도록 / only - 다른서버 응답X -> 나도 응답 X
    forwarders {네임서버 주소; 네임서버 주소;} 도메인에 대한 질의를 다른 서바로 넘길 때 사용하는 옵션
    allow-query { 네임서버 주소 }: 네임서버에 질의할 수 있는 호스트 지정, 복수 지정 가능
    allow-transfer { 네임서버 주소 }: zone 파일의 내용을 복사할 대상에 제한을 걸 때 지정
    datasize 512M: DNS 관련 정보를 캐싱으로 하는 데 사용하는 메모리 제한시
    recursive yes; 하위 도메인에 대한 검색 가능 여부를 지정하는 항목

    예시들
    zone "도메인명" IN {
        type master;
        file "존파일명"
    }

    zone "samadal.com" IN {
        type master;
        file "samadal.com.zone"
    }

    zone "30.168.192.in-addr.arpa" IN {
        type master;
        file "samadal.com.rev"
    }
    type 
    A: ipv4
    AAAA: ipv6
    NS: 도메인의 네임서버
    MX: 메일서버
    CNAME: 별칭
    PTR: 리버스 존에서만 사용. IP를 도메인으로 변환하기 위해서
    
    KVM
    service libvirtd start
    virt-manager

    * xinetd: inetd 대체를 위해서 등장
    log_type
    ex) log_type = SYSLOG daemon info
    ex) log_type = FILE /var/log/xinetd.log

    log_on_failure = 속성값 : 접속 실패시 기록될 속성값
    ex) log_on_failure HOST
    ex) log_on_failure USERID
    ex) log_on_failure ATTEMPT

    log_on_success = 속성값 : 접속 실패시 기록될 속성값
    
    cps = 초당요청수 제한시간 : 초당 요청받은 수에 대한 제한을 설정
    cps = 50 10 (초당 요청수가 50개 이상일 경우, 10초동안 접속을 중단한다)

    instances = 서버 최대 갯수 (동시 서비스 최대갯수)
    
    per_source = 특정 IP주소에서 접속할 수 있는 최대 개수

    only_from = IP 주소 / 네트워크 대역 주소  
    ex) only_from = 192.168.5.13 192.168.6.0 192.168.12.0/24

    no_access = IP 주소 / 네트워크 대역주소
    
    enabled = 사용가능한 서비스 목록
    ex) enabled = telnet ssh
    
    disabled = 특정 서비스 막을 때
    ex) disabled = telnet ssh

    좀더 자세한 사항 -> /etc/xinetd.d 디렉터리 안에 파일 생성
    
    socket_type = 속성값
    stream: stream기반
    dgram: datagram 기반
    raw: IP 기반 직접 접근

    wait = no | yes (no: 다중스레드, yes: 단일스레드)

    user = root (서버 프로세스의 uid 지정)

    server = /usr/sbin/in.telnetd : 데몬 파일 경로
        
    log_on_failure += USERID -> 로그 관련 설정으로 기본 설정에 추가 삭제할때

    access_times = 01:00-17:00 -> 지정된 시간에만 서비스 오픈

    redirect = 192.168.30.128 23 : 해당 서비스를 다른 서버로 포워딩할때 사용하는 항목
    
    port = 8080 : 해당서비스의 포트 번호 지정할때 사용하는 항목 
    
    nice = 10 : 서버의 우선순위 지정하는 항목    (-20 ~ 19)


* 프록시 관리 (SQUID)


    환경설정 파일: /etc/squid/squid.conf

    http_port 3128: 기본 포트값 - 3128

    acl 설정
    acl (별칭) (src) (IP 주소/넷마스크값)
    acl (별칭) (dst) (IP 주소/넷마스크값)

    ex) acl {} srcdomain .samadal.com
    ex) acl {} dstdomain .samadal.com

    http_access allow 별칭
    http_access deny

    특정 대역만 살리기
    acl samadal src 192.168.4.0/255.255.255.0
    http_access allow samadal
    http_access deny all

    특정 대역만 죽이기
    acl samadal src 192.168.4.0/255.255.255.0
    http_access deny samadal
    http_access allow all
    

* DHCP 관리 - 클라이언트에서 IP, GW, NameServer 주소 등을 할당해주는 서비스

    
    /etc/dhcp.conf
    range 
    range dynamic-bootp
    option domain-name
    option domain-name-servers
    option routers
    option broadcast-address
    default-lease-time
    max-lease-time

    ex)
    subnet 192.168.1.0 netmask 255.255.255.0 {
        range 192.168.1.2 192.168.1.254;
        option domain-name "samadal.com";
        option domain-name-server ns.samadal.com;
        option routers 192.168.1.1;
        option broadcast-address 192.168.1.255;
        default-lease-time 600;
        max-lease-time 7200;
    }

    host namadal_pc {
        hardware ethernet 08:00;70:26:c0:a5;
        fixed-address 192.168.1.22;
    }    

    NTP 관리 (컴퓨터간 시간 동기화) 
    driftfile /var/lib/ntp/drift 지역시스템 시간을 정확하게 유지하는 파일로 지정하는 항목    
    restrict 192.168.1.0 mask 255.255.255.0 nomodify notrap : NTP 서버에 접근 제한 할때 
    server time.bora.net : 기준 NTP 서버 지정


#### 네트워크 보안

    iptables
    iptables [-t table] action chain match [-j target]

    -t: 테이블 지정
    action: 사슬 지정 (주로 -N / -A )
    chain: 사슬명기 (INPUT OUTPUT..)
    match: (-d -p)
    -j target: j로 옵션 지정


    action 
    -N: 새로운 사용자 정의 사슬 만들기
    -X: 비어있는 사슬 제거
    -P: 사슬의 기본정책 설정
    -L: 현재 사슬의 규칙 나열
    -F: 사슬로부터 규칙 제거
    -A: 새로운 규칙 추가
    -ㅣ: 사슬에 규칙 맨 첫부분에 추가
    -R: 사슬 규칙 교환
    -D: 사슬 규칙 제거

    chain
    INPUT
    OUTPUT
    FORWARD
    PREROUTING
    PORTROUTING

    match
    -s 
    -d 
    -p
    -i
    -o
    !   

    target
    ACCEPT
    DROP
    LOG
    REJECT
    RETURN

    예시
    ex) iptables -L INPUT: input 사슬에 설정된 정책 정보 출력
    ex) iptables -t nat -L: nat 테이블에 설정된 정책 정보 출력
    ex) iptables -F: 기본 테이블인 Filter의 모든 사슬에 설정된 정책 제거
    ex) iptables -A INPUT -s 192.168.12.22 -d localhost -j DROP
    ex) iptables -A INPUT -s 192.168.1.0/24 -j ACCEPT
    ex) iptables -A INPUT -s 192.168.5.13 -p icmp -j DROP
    ex) iptables -A INPUT -s 192.168.5.12 ! -p icmp -j DROP
    

    iptables 규칙 저장
    iptables-save > firewall.sh
    
    iptables 규칙 적용
    iptables-restore < firewall.sh

    service iptables save | start | stop | restart
    
    -> /etc/sysconfig/iptables에 저장
    
    Nat 구현
    SNAT - 발신지 변경
    ex) iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to 203.247.50.3 
    ex) iptables -t nat -A PORTROUTING -o etho0 -j SNAT --to 203.247.50.3-203.247.50.7

    DNAT - 도착지 변경
    ex) iptables -t nat -A PREROUTING -p tcp -d 203.247.50.3 --dport 80 -j DNAT --to 192.168.1.11:80
    ex) iptables -t nat -A PREROUTING -p udp -d 203.247.50.3 --dport 53 -j DNAT --to 192.168.1.10:53


    

    


    