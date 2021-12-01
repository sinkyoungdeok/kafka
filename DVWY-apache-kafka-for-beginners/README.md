
# 1. 아파치 카프카 개요 및 설명 
<details><summary> 자세히보기 </summary>

### 카프카가 없을 땐

1. 소스1 <--> 타겟1 단방향 통신

![image](https://user-images.githubusercontent.com/28394879/144202193-8d8d1314-3035-41f4-9aee-074ff759f4c0.png)

2. 소스N <--> 타겟M 굉장히 많은 단방향, 양방향 통신들

![image](https://user-images.githubusercontent.com/28394879/144202449-5bf61b65-ebb2-4d56-ad8b-324325bd53c5.png)


**1번 구조에서 2번구조로 점점 되가면서 생기는일들**
- 데이터 전송 라인이 많아 짐에 따라, 배포와 장애에 대응하기 어려워졌다.
- 데이터를 전송할 때 프로토콜 포멧의 파편화가 심각해졌다.
- 추후에 데이터의 포멧내부의 변경사항이 있을 때 유지보수가 힘들다


### 카프카 이후
![image](https://user-images.githubusercontent.com/28394879/144202817-6534b6cc-cf27-4156-9984-b1f5d70db790.png)
- 복잡함을 해결하기 위해 링크드인에서 내부적으로 개발
- 현재는 오픈소스로 제공 

### 카프카 특징
![image](https://user-images.githubusercontent.com/28394879/144203176-d15f48d1-7686-4e9d-bcb2-b3b4d7e12792.png)
- 소스 애플리케이션과 타겟 애플리케이션의 커플링을 약하게 해줌
- 소스 애플리케이션은 아파치 카프카에 데이터를 전송하면 된다.
- 타겟 애플리케이션에서는 아파치 카프카에서 데이터를 가져오면 된다.
- 예) 쇼핑몰
  - 소스 애플리케이션 
    - 클릭 로그
    - 결제 로그
  - 타겟 애플리케이션
    - 로그 적재
    - 로그 처리
- 소스 애플리케이션에서 보낼 수 있는 데이터형식은 거의 제한이 없다.


![image](https://user-images.githubusercontent.com/28394879/144203636-a14d31d6-bb2a-4c9f-9702-cd1d7f8a92da.png)
- 토픽
  - 하나의 토픽이 하나의 큐라고 생각하면된다.
- Producer
  - 데이터를 송신
- Consumer
  - 데이터를 수신
 

### 카프카의 장점 
- fault tolerant
  - 고가용성으로 서버가 이슈 생기거나 갑작스럽게 랙(전원이) 내려가는 상황에서도 데이터를 손실 없이 복구할 수 있다.
- 낮은 지연(latency)와 높은 처리량(Throughput)를 통해서 아주 효과적으로 데이터를 많이 처리할 수 있다.
  - 빅데이터 처리에서는 거의 무조건 사용하고 있다.


</details>

# 2. 토픽이란?
<details><summary> 자세히보기 </summary>

### 카프카 토픽 특징
![image](https://user-images.githubusercontent.com/28394879/144206455-f96f3955-b071-48b9-a54b-93db52318b23.png)

- 카프카에서는 토픽을 여러개 생성할 수 있다.
- 토픽은 데이터베이스의 테이블이나 파일시스템의 폴더와 유사한 성질을 가지고 있다.
- Producer가 데이터를 가져가고 Consumer는 데이터를 가져가게 된다.
- 토픽은 이름을 가질 수 있는데 목적에 따라 무슨 데이터를 담는지 명확하게 명시하면 추후 유지보수 시 편리하게 관리할 수 있다.


### 카프카 토픽 내부 (파티션이 1개 일 경우)
![image](https://user-images.githubusercontent.com/28394879/144207010-bc5d1494-663d-4bbf-b814-1ed63504f906.png)

- 하나의 토픽은 여러개의 파티션으로 구성될 수 있다.
- 첫번째 파티션 번호는 0번부터 시작한다.
- 하나의 파티션은 큐와 같이 내부에 데이터가 파티션끝에서부터 차곡차곡 쌓인다.
- 클릭로그 토픽에 카프카 컨슈머가 붙게되면 데이터를 가장 오래된 순서대로 가져간다. (0번부터 6번까지)
- 더이상 데이터가 들어오지 않으면 컨슈머는 또 다른 데이터가 들어올때까지 기다린다.
- 컨슈머가 record들을 가져가도 데이터는 삭제되지 않는다.
- 새로운 컨슈머가 붙었을 때 다시 0번부터 가져가서 사용할 수 있다.
  - 다만 컨슈머 그룹이 달라야 하고, auto.offset.reset=earlieast로 설정되어 있어야 한다.
- 동일데이터를 두번 처리할 수 있는데 이게 카프카를 사용하는 아주 중요한 이유기도 하다.
 

### 카프카 토픽 내부 (파티션이 2개이상 일 경우)
![image](https://user-images.githubusercontent.com/28394879/144208476-7b8e7c66-fd5d-4803-801d-9a556496b147.png)

1. 키가 null 이고, 기본 파티셔너 사용할 경우
   - 라운드 로빈(Round robin)으로 할당
2. 키가 있고, 기본 파티셔너 사용할 경우
   - 키의 해시(hash)값을 구하고, 특정 파티션에 할당

- 파티션을 늘리는것은 조심해야 한다. 
  - 파티션을 늘릴 수는 있지만, 파티션을 다시 줄일 수 없기 떄문이다.
- 파티션의 레코드는 언제 삭제되나? 옵션에 따른다.
  - log.retentions.ms: 최대 record 보존 시간
  - log.retentions.byte: 최대 record 보존 크기(byte)


</details>

# 3. 브로커, 복제, ISR(In-Sync-Replication)
<details><summary> 자세히보기 </summary>

</details>

# 4. 파티셔너(Partitioner)란?
<details><summary> 자세히보기 </summary>

</details>

# 5. 컨슈머 랙(Consumer Lag)이란?
<details><summary> 자세히보기 </summary>

</details>

# 6. 컨슈머 랙 모니터링 애플리케이션, 카프카 버로우(Burrow)
<details><summary> 자세히보기 </summary>

</details>
