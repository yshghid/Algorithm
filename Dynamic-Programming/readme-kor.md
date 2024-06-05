## 알고리즘 패러다임

1. 탐욕 알고리즘 (Greed): 입력을 특정 순서로 처리하여, 각 단계에서 지역적으로 최적인 결정을 내린다.
2. 분할 정복 (Divide-and-Conquer): 문제를 하위 문제로 분할하여, 각 하위 문제를 개별적으로 해결한 후, 그 해결책을 결합하여 원래 문제를 해결한다. 합병 정렬(merge sort)와 퀵 정렬(quicksort) 등이 있다.
3. 동적 프로그래밍 (Dynamic Programming): 문제를 겹치는 하위 문제 시리즈로 분해하여, 각 하위 문제의 해결책을 결합하여 더 큰 문제를 해결한다.

## 가중치가 있는 구간 스케쥴링

- 작업 j는 시작 시간 sj, 종료 시간 fj, 가중치 wj를 갖는다.
- 상호 호환 가능한 작업들 중 최대 가중치의 부분 집합을 찾는다. ("호환": 두 작업이 겹치지 않음)

### 동적 프로그래밍의 이진 선택 (Dynamic Programming: Binary Choice)

- OPT(j): 첫번째부터 j번째 작업까지 호환 가능한 작업들 중 최대 가중치
- OPT(j) = max(OPT(j-1),OPT(p(j)+wj)
- 두 선택지 중 하나를 선택: j번째 작업을 선택하지 않으면 OPT(j-1)가 최적의 해. j번째 작업을 선택하면 j번째 작업과 호환 가능한 마지막 작업 p(j) 까지의 최대 가중치 OPT(p(j))에 wj를 더한 값이 최적의 해.

#### 슈도코드

```
BRUTE-FORCE (n, s1, …, sn, f1, …, fn, w1, …, wn)
Sort jobs by finish time and renumber so that f1 ≤ f2 ≤ … ≤ fn.
Compute p[1], p[2], …, p[n] via binary search.
RETURN COMPUTE-OPT(n).

COMPUTE-OPT(j)
IF (j = 0)
  RETURN 0.
ELSE
  RETURN max {COMPUTE-OPT(j – 1), wj + COMPUTE-OPT(p[j])}.
```
BRUTE-FORCE
- 정렬: 종료 시간 fj에 따라 작업을 정렬
- 이진 탐색: 작업 j와 호환되는 마지막 작업 p[j]를 계산

COMPUTE-OPT
- 재귀적 호출: j–1와 p[j]에 대해 COMPUTE-OPT를 다시 호출. 각 j에 대해 최댓값 선택.

시간 복잡도
- BRUTE-FORCE: 정렬 O(nlogn), 이진 탐색 O(nlogn)
- COMPUTE-OPT: 재귀적 호출 O(2^n)
- 최악의 경우(worst case) 총 시간 복잡도: O(2^n), COMPUTE-OPT에 의해 지배됨. 

### 메모이제이션 (memoization)

- 동적 프로그래밍에서 하향식(top-down) 접근 방식을 사용할 때 중복 계산을 피하기 위해 이미 해결된 부분 문제의 결과를 저장하고 재사용한다.

#### 슈도코드
```
TOP-DOWN(n, s1, …, sn, f1, …, fn, w1, …, wn)
Sort jobs by finish time and renumber so that f1 ≤ f2 ≤ … ≤ fn.
Compute p[1], p[2], …, p[n] via binary search.
M[0] ← 0.
RETURN M-COMPUTE-OPT(n).

M-COMPUTE-OPT(j)
IF (M[j] is uninitialized)
  M[j] ← max {M-COMPUTE-OPT (j – 1), wj + M-COMPUTE-OPT(p[j])}.
RETURN M[j].

FIND-SOLUTION(j)
IF (j = 0)
  RETURN ∅.
ELSE IF (wj + M[p[j]] > M[j – 1])
  RETURN {j} ∪ FIND-SOLUTION(p[j]).
ELSE
  RETURN FIND-SOLUTION(j – 1).
```

TOP-DOWN
- 정렬: 종료 시간 fj에 따라 작업을 정렬
- 이진 탐색: 작업 j와 호환되는 마지막 작업 p[j]를 계산

M-COMPUTE-OPT
- 동적 프로그래밍: j–1와 p[j]에 대해 M-COMPUTE-OPT를 호출. M[j]를 계산

FIND-SOLUTION
- M[j] 역추적: j–1와 p[j]에 대해 M[j]를 호출. 각 j에 대해 최댓값 선택.

시간 복잡도
- TOP-DOWN: 정렬 O(nlogn), 이진 탐색 O(nlogn)
- M-COMPUTE-OPT: 동적 프로그래밍 O(n)
- FIND-SOLUTION: M[j] 역추적 O(n)
- 최악의 경우 총 시간 복잡도: O(nlogn), TOP-DOWN에 의해 지배됨.


