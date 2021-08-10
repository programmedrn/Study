# Quartz 테이블 정리
## 개념
1. quartz는 in-memory 방식과 database 방식이 있다.
	 - in memory방식 : quartz가 수행할 job의 정보를 memory에 기록
	 - database 방식 : job의 정보를 db에 기록하여 재기동 시에도 작업을 이어갈 수 있음
## 장점과 단점
- 장점
	1. Clustering 지원 : 시스템 fail over와 random 로드 분산
	1. 여러 기본 plug ins : jvm 종료를 캐치 / job 실행 로그
- 단점
	1. Clustering이 random방식 뿐
	1. UI 미제공
	1. 스케줄링 실행에 대한 History가 없음
	1. Fixed Delay가 없어 추가 작업이 필요
## 용어 정리
- Job : execute(JobExecutionContext context) 인터페이스. 실제 작업을 해당 메서드에 구현
- JobDetail : Job 실행 정보를 담는 객체. 이름, 그룹, DataMap을 설정. Trigger가 수행하는 기준
- JobDataMap : Job인스턴스 실행 시 사용할 정보를 담는 객체. JobDetail과 함께 세팅하면 됨
- Trigger : Job의 실행 조건을 담는 객체. N Trigger, 1 Job. 2가지 형태가 있다.
	- SimpleTrigger : 특정 시간에 Job 수행. 반복횟수와 실행 간격 지정 가능
	- CronTrigger : cron표현식으로 정의.
- MisfireInstruction : 실행되지 못한 불발 Trigger에 대한 정의. Scheduler가 종료될 때 혹은 가용 Thread가 없을 때 발생.
	- FIRE NOW : 바로 실행
	- DO NOTHING : 아무것도 하지 않음  
	등의 실행 규칙이 있다.
- Listener : Scheduler의 event를 받는 interface로 두 가지가 있다.
	- JobListener : Job 실행 전후에 대한 이벤트
	- TriggerListener : Trigger 발생하거나 불발 때, Trigger 완료 때 이벤트
- JobStore : Job과 Trigger의 정보를 2가지 방식으로 저장
	- RamJobStore : 메모리에 스케줄 정보 저장. 성능 우수
	- JDBC JobStore : DB에 저장. 재기동 안정.
	- 다른 JobStore로 Redis나 MongoDB도 사용 가능
## 실행 순서
SpringIoC가 SchedulerFactoryBean 실행(설정 세팅) > Scheduler 생성 > QuartzScheduler 시작 > JobStore 설정에 따라  
ClusterManagerThread 혹은 MisfireHandlerThread 실행 > SchedulerThread run() > JobListener, TriggerListener 실행
## Database 방식
### 현재 존재하는 테이블
* qrtz_cron_triggers
* qrtz_fired_triggers
* qrtz_job_details
* qrtz_locks
* qrtz_paused_trigger_grps
* qrtz_scheduler_state
* qrtz_simple_triggers
* qrtz_triggers
### 테이블 연동
- DB에 사용할 테이블들을 만들어 둔다.
	- 주의 : 같은 quartz 시스템에 사용할 테이블들은 모두 동일한 머릿말을 가지고 있어야 하며  
머릿말을 제외한 부분(예 : simple_triggers)은 규칙을 따라야 한다.
	- 이후 Spring 설정에서 quartz.jobStore.tablePrefix로 해당 머릿말의 정보를 준다. 여기서는 qrtz_
### 테이블별 용도
1. triggers : 트리거들의 이름, 그룹, Job정보, 설명, 다음 수행 시간, 이전 수행 시간, 트리거 상태, 시작 시간, 종료 시간, 불발 해소 전략, JobData 저장
1. simple_triggers : 단순 반복 trigger의 횟수, 간격 저장 
1. cron_triggers : Cron Trigger와 cron 표현식 저장 / 
1. locks : 스케줄 이름, 락 이름 연결 / 어플리케이션의 pessimistic lock 정보 저장
1. job_details : 스케줄 이름, Job 이름, 그룹, durable한지, nonconcurrent인지, updatedata인지, 등 저장 / 모든 구성된 Job의 detail 저장
1. simprop_triggers : 스케줄 이름, trigger 이름, 그룹, props 저장 / 
1. fired_triggers : 트리거 이름, 그룹, 인스턴스명, fired 시간, 스케줄된 시간, 상태, job 이름, 그룹, noncuncurrent한지 저장 / 발동 trigger와 관련 실행 정보 저장
1. scheduler_state : 스케줄명, 인스턴스명, 마지막 check in 시간, check in 간격 저장 / 스케줄러와 스케줄러 인스턴스의 상태 정보 저장
1. paused_trigger_grps : 스케줄명, 트리거 그룹 / 중지된 trigger 그룹 저장
1. calendars : 스케줄 이름, 달력 이름, 달력 / 쿼츠 calenedar 정보를 저장

   
  
## 참조
- Scheduler의 Job추가시 재배포 해야 하는 문제 개선 :   
https://homoefficio.github.io/2019/09/28/Quartz-%EC%8A%A4%EC%BC%80%EC%A4%84%EB%9F%AC-%EC%A0%81%EC%9A%A9-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98-%EA%B0%9C%EC%84%A0-1/
- 실행 로직 :  
https://blog.advenoh.pe.kr/static/4c2d16897ac34e462c2d1753d9293d9a/b2dbf/image_8.png
- 테이블별 칼럼 :   
https://i.stack.imgur.com/20omd.png
- 테이블별 역할 :  
https://flylib.com/books/en/2.65.1/creating_the_quartz_database_structure.html





