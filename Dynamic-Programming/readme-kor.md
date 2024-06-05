# Lec9. Dynamic Programming

- 알고리즘 패러다임

1. 탐욕 알고리즘 (Greed): 입력을 특정 순서로 처리하여, 각 단계에서 지역적으로 최적인 결정을 내린다.
2. 분할 정복 (Divide-and-Conquer): 문제를 하위 문제로 분할하여, 각 하위 문제를 개별적으로 해결한 후, 그 해결책을 결합하여 원래 문제를 해결한다. 합병 정렬(merge sort)와 퀵 정렬(quicksort) 등이 있다.
3. 동적 프로그래밍 (Dynamic Programming): 문제를 겹치는 하위 문제 시리즈로 분해하여, 각 하위 문제의 해결책을 결합하여 더 큰 문제를 해결한다.

## 가중치가 있는 구간 스케쥴링

- 작업 j는 시작 시간 sj, 종료 시간 fj, 가중치 wj를 갖는다.
- 상호 호환 가능한 작업들 중 최대 가중치의 부분 집합을 찾는다. ("호환": 두 작업이 겹치지 않음)

### 동적 프로그래밍 - 이진 선택 (Binary Choice)

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


## 세그먼티드 최소 제곱법 (Segmented Least Squares)

### 최소 제곱법
<img src="https://github.com/yshghid/Algorithm/assets/153489198/4b50581c-7dd8-4ee0-8b16-7106b3d091b4" width=600>

- 위의 주어진 데이터 포인트에 가장 잘 맞는 직선 f(x) 찾기: 데이터 포인트와 직선 사이의 오차를 최소화해서 찾는다.
- "오차": SSE(Sum of Squared Errors)
- 구하면 아래와 같음.
<img src="https://github.com/yshghid/Algorithm/assets/153489198/adbb9beb-391c-42c3-9c8c-0b557bc18c64" width=600>

### 세그먼티드 최소 제곱법

<img src="https://github.com/yshghid/Algorithm/assets/153489198/50b6e71a-d517-4719-9b4f-2805ec13cc8f" width=600>

- 위와 같이 데이터 포인트가 시간에 따라 다른 트렌드를 보이는 경우 각 트렌드를 여러 직선 세그먼트로 나타내는 함수가 주어진 데이터 포인트에 가장 잘 맞는다.
- 위의 주어진 데이터 포인트에 가장 잘 맞는 함수 f(x) = E + cL 찾기: 모든 세그먼트의 제곱 오차의 합 E와 사용된 세그먼트 수 L의 합을 최소화해서 찾는다.

<img src="https://github.com/yshghid/Algorithm/assets/153489198/c46da75e-5ad1-4cdc-b877-aaf745a1e2e1" width=300>

- 전체 데이터 포인트: p1-pj
- OPT(i-1): i-1번째 데이터 포인트까지에 대해 계산된 최소 비용.
- eij: i번째 데이터 포인트부터 j번째 데이터 포인트까지의 최소 오차. 즉, 마지막 세그먼트의 SSE
- c: 세그먼트 추가에 드는 고정 비용
- OPT(j) = min(1≤i≤j){OPT(i−1) + eij + c}

#### 슈도코드

```
SEGMENTED-LEAST-SQUARES(n, p1, …, pn, c)
FOR j = 1 TO n
  FOR i = 1 TO j
    Compute the SSE eij for the points pi, pi+1, …, pj.
M[0] ← 0.
FOR j = 1 TO n
  M[j] ← min 1 ≤ i ≤ j { eij + c + M[i – 1] }.
RETURN M[n].
```
- SSE 계산: n개의 데이터 포인트가 있을때 세그먼트의 시작과 끝 (i,j)의 모든 조합에 대해서 (pi, pi+1, ..., pj)의 SSE 계산
- M[j] 계산: 현재 끝 j에 대해 가능한 모든 i에 대해 해당 세그먼트의 SSE(eij), 세그먼트 추가 비용(c), 그리고 이전 점까지의 최소 비용(M[i-1])의 합이 최소가 되는 값을 M[j]로 할당한다.
- 시간 복잡도: SSE 계산 O(n^3), M[j] 계산 O(n^2)

## Knapsack 문제

- 아이템 집합 I가 있을때 각 아이템은 가치 vi와 무게 wi를 갖는다. 배낭의 무게 한도 내에서 가장 높은 가치를 담을 수 있는 아이템 집합을 선택하기.

그리디 알고리즘?

- Greedy-by-value(아이템을 가치가 높은 순으로 선택): 배낭의 무게 한도를 고려하지 않으므로 최적해를 보장하지 않음.
- Greedy-by-weight(가장 가벼운 아이템부터 선택): 가벼운 아이템의 가치가 낮을 경우 비효율적일 수 있음.
- Greedy-by-ratio(가치 대비 무게 비율이 가장 높은 아이템을 선택): 평균적으로 좋은 성능을 보이지만, 가방의 정확한 무게 한도를 맞추어 최적의 가치를 달성하는 것은 보장하지 못함.
- Greedy 알고리즘은 Knapsack 문제의 최적해를 보장하지 못할 때가 많음.

동적 프로그래밍으로 풀 경우 하위 문제?

- OPT(w): 배낭의 무게 한도가 w일 때 담을 수 있는 아이템들의 최대 가치.
- OPT(i): 처음 i개의 아이템만 고려할 때의 배낭 문제의 최적 가치.
- OPT(i, w): 처음 i개의 아이템과 무게 한도 w를 고려하는 배낭 문제의 최적 가치. 

### 동적 프로그래밍 - 두가지 변수(Two variables)

- OPT(i,w): 처음 i개의 아이템과 무게 한도 w를 고려할 때의 최적 가치
- OPT(i,w) = max(OPT(i−1,w),vi + OPT(i−1,w−wi))

#### 슈도코드
```
KNAPSACK(n, W, w1, …, wn, v1, …, vn )
FOR w = 0 TO W
  M[0, w] ← 0.
FOR i = 1 TO n
  FOR w = 0 TO W
    IF (wi > w) M[i, w] ← M[i – 1, w].
    ELSE M[i, w] ← max { M[i – 1, w], vi + M[i – 1, w – wi] }.
RETURN M[n, W].
```
- 무게 고려: wi > w: 재 아이템의 무게가 현재 무게 한도보다 크면 M[i-1, w] 를 유지.
- 가치 고려: 아이템 i를 포함하지 않는 경우의 최대 가치 M[i-1, w]. 아이템 i의 무게를 제외한 무게 한도(w-wi)에서의 최대 가치 + 현재 아이템의 가치 M[i-1, w-wi] + vi. 둘중 더 큰 값 max{M[i-1, w], vi + M[i-1, w-wi]}을 M[i, w]에 저장.
- 시간 복잡도: 무게 고려, 가치 고려 Θ(nW)
- 공간 복잡도: M[i, w] Θ(nW)

중요한 가정
- 무게가 정수라는 가정: 이 알고리즘은 무게가 정수 값이라는 점에 크게 의존하며, 무게가 정수가 아닐 경우 무게 한도를 정확히 맞추기 위한 인덱싱이 복잡해지거나 불가능해진다.
- 가치가 정수일 필요는 없음: 가치 값에 대한 가정은 명시적으로 사용되지 않는다.

## 시퀀스 정렬 (Sequence Alignment)
