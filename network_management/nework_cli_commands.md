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


    