### File management comand lines
<hr>

#### file, directory management

  - Set-UID(4000)rws/Srwxrwx, Set-GID(2000) rwxrws/Srwx, Sticky-bit(1000) rwxrwxrwt/T
    <br>읽기권한: 4, 쓰기권한: 2, 실행권한: 1

    
#### file system recovery
  - 인식확인: fdisk -l
  - 파티션 분할 및 생성: fdisk /dev/sdb
  - 파일시스템 생성: mkfs.ext4 /dev/sdb
  - 사용 디렉터리 생성: mkdir /lee
  - 마운트 명령: mount
  - 관련 파일 등록: /etc/fstab
    
    
    fdisk -l # 파티션 테이블 정보 출력
    fdisk -s # 파티션 크기 특정 block_size
    fdisk -v # 버전출력

    fdisk -p # 현재 디스크 정보 출력
    fdisk -d # 파티션 삭제
    fdisk -n # 파티션 새롭게 추가
    fdisk -t # 파티션 속성 변경 (82: swap, 83: linux, 8e: Linux LVM, fd: Raid) 
    fdisk -w # 저장하고 종료
    fdisk -q # 저장하지 않고 종료 
    

    #새로운 파일 시스템 만들기
    mkfs -t (파일시스템 유형) -c (배드블록 체크) -v (자세히 출력) {대상}
    mke2fs -j (저널링파일시스템 -> ext3) -t (타입) -b (블록 사이즈) {대상}
    
    # 마운트 옵션
    mount -a (/etc/fstab에 명시된 경우) -t (유형) -o (추가 설정) {대상}
    ex) 
    * mount -t ext4 -o ro /dev/sdb1
    * mount -o remount /home
    * mount -t smbfs -o username=lee,password='1234' //192.168.30.128/date net 
    
현재 시스템에 마운트 되어 있는 파일 시스템 정보 -> /etc/mtab

/etc/fstab 내용?
(장치명) | (마운트 디렉터리) | (파일시스템) | (마운트 옵션) | (dump 백업설정) | (부팅 체크 옵션)

#### 마운트 옵션? 
  * default: rw, suid, dev, exec, auto, nouser, async
  * auto:
  * noauto:
  * user:
  * usrquota:
  * grpguota:
  * noquota:
  * suid:
  * nosuid:
  * nodev:
  * noexec:
  * ro:
  * rw:
  * async:
  * acl: 
    
