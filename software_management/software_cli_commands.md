### software management command lines
<hr>

#### software install and management

    * 유형: rpm, yum, dpkg, apt-get (man 이용 필요)

    rpm (옵션) (패키지 파일명)
    -i: 새로운 패키지 설치
    -U: 새로운 버전으로 업그레이드
    -F: 이전 버전이 설치되어 있는 경우에만 설치
    -h: 설치 상황을 ###로 표기
    --force: 강제로 설치할 때
    --nodeps: 의존성 관계 무시 설치
    --test: 제대로 되는지 테스트
    --rebuilddb: rpm db 업데이트

    -e: 설치된 패키지 삭제
    --nodeps: 의존성 있어도 삭제
    --test: 테스트
    --allmatches: 중복시 삭제 
    
    ex) rpm -e httpd --nodeps (http패키지 제거, 의존성 있어도 제거) 

    -q: 질의 옵션
    -i: 설치된 패키지 정보 출력
    -l: 모든 파일 정보 출력
    -a: 패키지 목록 확인
    -p (패키지 파일명): 패키지 파일에 대한 정보 
    -f (파일명): 지정한 파일 설치한 패키지 이름 출력
    -c: 설정파일이나 스크립트 파일 출력
    -d: 문서 파일 출력
    -R: 의존성 보여줌
    --changelog: 변경내역 연대순
    --scripts: 설치 및 제거 관련 스크립트 보여줌
    --filesbypkg: 패키지 명 붙여주기
    --queryformat: 질의결과 원하는 포맷 있는 경우

    ex) rpm -qi sendmail
    ex) rpm -ql sendmail
    ex) rpm -qip () 
    ex) rpm -qlp (): 설치되는 파일 목록 정보 출력
    ex) rpm -qf (): 해당 파일 설치한 패키지 정보

    ex) rpm -Va (시스템에 설치된 모든 패키지 검증)

    ex) rpm --rebuild 
    ex) rpmbuild --rebuild : ().src.rpm 파일을 패키지로 만드는 모드


    # 소스코드 컴파일
    # 설치 프로세스: 압축풀기 - 디렉터리 이동 - configure - make - make install
    tar (옵션) (파일명)
    -c: 묶어서 파일생성
    -x: 파일 풀기
    -v: 대상이 되며, 파일들을 보여줌
    -f (파일명): 대상 파일명 지정
    -r: 기존 파일 뒤에 추가
    -t: tar 파일 안에 묶인 파일 목록 출력
    -C: 디렉터리 변경시 사용
    -p: 권한유지
    -Z: 압축옵션 compress 형식) tar.Z
    -z: 압축옵션 gzip 형식) tar.gz
    -j: 압축옵션 bzip2 형식) tar.bz2
    -J: 압축옵션 xz 형식) tar.xz

    
    
    