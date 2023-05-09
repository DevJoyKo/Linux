### Device management command lines
<hr>

#### 모듈 설치
 * 위치: /lib/modules/{version}/kernel


    lsmod: 적재된 모듈 정보 출력
    insmod: 모듈 적재 ex) insmod iptables.ko
    rmmod: 모듈 삭제 ex) rmmod iptables.ko
    modprobe: 적재하거나 삭제하는 명령 (의존성 있는 것 같이 적재) - modules.dep 참조
    
    ex) modprobe -l: 사용가능한 모듈 정보 출력
    ex) modprobe -r: 의존성 있는것까지 같이 삭제
    ex) modprobe -c: 모듈 관련 환경설정 파일 같이 출력

    modinfo: 모듈 파일에 대한 정보를 출력

    * 설정파일: /etc/modprobe.d 디렉터리의 .conf 
    * 의존성 파일: /lib/modules/{커널버전}
    
    depmod: 갱신관리


#### 커널 설치 
    - 설정값 초기화: make mrproper(설정파일 삭제) (유의) make clean, make distclean(모든 백업 등 전체 삭제)
    - 옵션 설정작업: make menuconfig / make xconfig / 
    - 커널 이미지 생성: make bzImage 
    - 컴파일 작업: make modules
    - 커널모듈 설치: make modules_install
    - 커널파일 복사, grub.conf 파일수정: make install


#### 프린터

    설정 정보 파일: /etc/printcap

    lpr (옵션) (파일명) -> 프린터 작업 요청
    -# n: 매수지정
    -m: 끝나면 이메일로 보내기
    -P 프린터명: 기본 프린터 외 지정
    -r: 출력 뒤 지정 파일 삭제

    lpq (옵션) -> 프린터 큐 작업 목록 출력
    -a: 모든 프린터 작업 정보 출력 
    -ㅣ: 출력결과 자세히 출력
    -P: 특정 프린터 지정

    lprm (옵션) (파일명) -> 대기 파일 삭제
    -: 모든 작업 취소
    -U: 특정 사용자 작업 취소
    -n: 인쇄 매수 지정
    -P: 프린터 지정
    -h: 지정 서버 작업 취소

    lpc -> 프린터나 프린터 큐를 제어할 때 사용

    lp (옵션) (파일명) -> lpr 명령과 유사
    -d: 다른 피린터 지정
    -n: 인쇄 매수 지정

    lpstat -> 프린터 큐 상태 출력
    -p: 인쇄가능 여부 출력
    -t: 상태 정보 출력





