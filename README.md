## Greenplum Disaster Recovery 이란?
- Greenplum DR를 사용하면, 재해 발생 전 특정 복구 시점으로 복구 가능
- Greenplum DR은 Full 백업/복구, Incremental 백업/복구, WAL 로그 기반으로 DR 기능 제공
- 스냅샷을 이용할 경우 PITR(Point-in-time-recovery) 수행 가능. 
- 링크: https://docs.vmware.com/en/VMware-Greenplum-Disaster-Recovery/index.html
- Greenplum7.3+에서는 DR 클러스터에서 조회기능을 제공. (Greenplum 6에서는 미제공)

### 1. GPDR 아키텍처
```
GP Primary                  Repository                      GP Recovery 
  Cluster                                                     Cluster
            -------->       S3, NFS,            -------->  
            Backup          AWS, GCP, Azure,    Restore
            physical data   DataDomain          backups 
            and             Local Filesystem    and WAL     
            archive WAL
```

### 2. GPDR 지원 버전
- VMware Greenplum 6.27.1 and higher for RHEL 7.x, 8.x, and 9.x
- VMware Greenplum 7.3.0 and later for RHEL 8.x and 9.x  
- ==> Read replica(DR 조회기능)는 Greenplum 7.3 이후 지원   

### 3. 전제 조건
- Greenplum Primary 및 Mirror 호스트들이 레퍼지토리에 접속 가능해야 함.
- Greenplum Major이 같아야 함. (GP6<->GP6, GP7<->GP7)
- OS 버전도 같아야 함. 
- Primary Cluster의 모든 Role/User에 대해서는 Recovery Cluster에 존재해야 함.(Role은 별도로 생성 필요)
- 확장 패키지에 대해서도 Recovery Cluster에 존재해야 함.(확장 패키지는 별도로 생성 필요)   

### 4. Greenplum DR 테스트 환경
- Greenplum 7.3 Primary Cluster (싱글머신)
- Greenplum 7.3 Recovery Cluster (싱글머신)
- minio (싱글머신)
- 폴더 및 파일 설명
```
./gpconfigs                               : gpdr을 위한 설정 파일
./gpdr                                    : gpdr 테스트를 위한 스크립트
./minio                                   : minio background로 수행하기 위한 스크립트
1.GreenplumDR_install_config.txt          : Greenplum DR 설치 및 설정 방법
2.GreenplumDR_gpdr_test_script.txt        : Greenplum DR 테스트를 위한 스크립트 
3.GreenplumDR_gpdr_test_full_recovery.txt : Greenplum DR Full Recovery 테스트 및 output
4.GreenplumDR_gpdr_test_incr_recovery.txt : Greenplum DR Incremental Recovery 테스트 및 output
5.GreenplumDR_gpdr_test_cont_recovery.txt : Greenplum DR Continuous Recovery 테스트 및 output
```
