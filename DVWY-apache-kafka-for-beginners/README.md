
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
