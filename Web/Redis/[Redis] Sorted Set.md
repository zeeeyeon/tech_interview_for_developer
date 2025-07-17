[Redis] Sorted Set

## Sorted Set
<hr>

- 값에 점수(score)를 부여하여 정렬된 상태로 저장하는 자료구조
- 값의 정렬 상태 유지와 효율적인 순위 계산 기능
- Skip List + Hashtable (key-value)

## Sorted Set 내부 구조
<hr>

- Skip List
  - Sorted Set은 Skip List를 사용하여 구현
  - Skip List는 여러 레벨의 Linked List로 구성(=계층)
  - 0 레벨은 모든 요소를 포함
  - 상위 레벨은 하위 레벨의 일부 요소만 포함
  - 각 레벨은 확률 기반으로 정해짐 (동전 던지기 방식)
  - 특정 값의 랭킹은 노드에 간격(=span)을 추가, span은 해당 노드가 몇 개의 노드를 건너뛰는지 나타냄

[참고 - https://redisgate.kr/redis/configuration/internal_skiplist.php]



