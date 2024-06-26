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

최소 제곱법
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

- Levenshtein의 편집 거리 (Edit Distance): 두 문자열을 서로 같게 만들기 위해 필요한 최소 편집 횟수. ("편집": 문자의 삽입, 삭제, 교체)

- 정렬 M은 두 문자열에서 선택된 순서쌍 (xi, yj)의 집합이다.
- 문자열 정렬의 목표는 두 개의 문자열 사이의 최소 비용 정렬을 찾는 것이다. 정렬의 비용은 불일치 비용(αpq) + 갭 비용(δ) 으로 계산된다. 

<img src="https://github.com/yshghid/Algorithm/assets/153489198/7a16abc7-bb30-47bc-9ef5-46596e39e921" width=600>

- 위 그림에서, 두 정렬 "C T – G A C C T A C G"와 "C T G G A C G A A C G"의 비용 cost = δ + αCG + αTA 이다.

<img src="https://github.com/yshghid/Algorithm/assets/153489198/b1b39d69-ac33-48ee-ad16-d4b11244bb6d" width=500> 

- 위 그림에서, 두 정렬 CTACCG 와 TACATG 의 최소 비용 정렬 M = { x2–y1, x3–y2, x4–y3, x5–y4, x6–y6 } 이다.

### 하향식(bottom-up) 알고리즘

<img src="https://github.com/yshghid/Algorithm/assets/153489198/8e710f1b-c311-42b3-b31a-6f139321ad81" width=600>

Q. 두 문자열 S=strong, T=stone 이 존재한다. 이때 stro, sto를 정렬하는 최소 비용 (OPT(4,3))은?
A. 세 경우 중 하나이다.

- 문자열 stro와 st의 정렬 비용 OPT(4,2) + 문자 o를 추가하는 비용
- 문자열 str와 sto의 정렬 비용 OPT(3,3) + 문자 o를 버리는 비용(이 경우 문자가 일치하므로 비용이 없음)
- 문자열 str와 st의 정렬 비용 OPT(3,2) + 문자 o를 다른 문자 o로 교체하는 비용(교체 비용이 없음)

#### 슈도코드
```
SEQUENCE-ALIGNMENT(m, n, x1, …, xm, y1, …, yn, δ, α)
FOR i = 0 TO m
  M [i, 0] ← iδ.
FOR j = 0 TO n
  M [0, j] ← jδ.
FOR i = 1 TO m
  FOR j = 1 TO n
    M [i, j] ← min{αxiyj + M[i–1,j–1], δ + M[i–1,j], δ + M [i,j–1]}.
RETURN M [m, n].
```
- M[i,j] 계산: xi 또는 yj가 갭으로 처리될 경우, 갭 비용 δ을 이전 행 또는 열의 값(M[i–1, j] 또는 M[i, j-1]에 더한 값이 가능하다. xi와 yj가 매치되고 불일치할 경우 αxiyj + 이전 대각선 셀의 값(M[i–1, j–1]) 이 가능하다. 셋 중 최솟값이 할당된다.
- 시간 복잡도: M[i,j] 계산 O(mn) (m,n: 두 문자열의 길이)

## 음수 가중치 유향 그래프

- 노드 v에서 t로의 경로에 음수 사이클 W가 포함되어 있다면, 최단 경로는 존재하지 않는다. 음수 사이클 W를 무한히 순환하면서 경로의 길이를 무한히 줄일 수 있기 때문이다.
- 그래프에 음수 사이클이 없다면, 모든 정점 v에서 t로의 최단 간단 경로(반복되는 정점이 없는 경로)가 존재하며, 이는 최대 n-1개의 간선을 포함한다. 경로에 포함된 사이클을 제거하더라도 경로 길이는 늘어나지 않는다.

음수 가중치 그래프 문제
- 음수 가중치를 포함한 최단 경로 찾기
- 음수 사이클 감지하기

<img src="https://github.com/yshghid/Algorithm/assets/153489198/5920a692-9f9e-45a7-a418-ee89ab5cc6af" width=600>

### 최단 경로 찾기

Q. 음수 가중치 유향 그래프에서, 각 노드 v에서 t까지 최단 경로를 찾기 위한 하위 문제?
A. 
- OPT(i, v) = 정확히 i개의 간선을 사용하는 v에서 t까지의 최단 경로의 길이 -> x
- OPT(i, v) = 최대 i개의 간선을 사용하는 v에서 t까지의 최단 경로의 길이 -> o. 이 정의는 Bellman-Ford 방식의 반복적인 에지 완화 과정과 일치하며, i번째까지의 간선을 사용하여 최단 경로를 점진적으로 구축하는 데 적합하다.

#### 슈도코드
```
SHORTEST-PATHS(V, E, ℓ, t)
FOREACH node v ∈ V :
  M [0, v] ← ∞.
M [0, t] ← 0.
FOR i=1 TO n–1
  FOREACH node v ∈ V :
    M [i, v] ← M [i–1, v].
    FOREACH edge (v, w) ∈ E :
      M[i, v] ← min{M[i, v], M[i–1, w] + ℓvw}.
```

<img src="https://github.com/yshghid/Algorithm/assets/153489198/38d6b1af-4dd7-480d-b018-a1c3abf06041" width=600>

- M[i,v] 계산: 방향이 v->k->w 라고 할때, v->k의 가중치(lvk)에 k->w의 최단 경로값(M[i-1,k])을 더한 값을 구해서, 현재 최단 경로값 M[i,v] 와 비교하여 더 작은 값을 선택한다.
- 시간 복잡도: M[i,v] 계산 O(nm) (m은 에지의 수, n은 노드의 수)

### 최단 경로 찾기 알고리즘 수정?
- Bellman-Ford 알고리즘의 시간 복잡도는 O(mn)(n개 노드에 대한 최단 경로 길이 계산), 공간 복잡도는 O(n)(n개 노드에 대한 최단 경로 길이 저장)이다.
- 경로 길이 계산: O(n)으로 가능.
- 최단 경로 저장: O(m+n)으로 가능.
- 두 개의 1차원 배열을 사용하기.

#### 슈도코드 2
```
BELLMAN–FORD–MOORE(V, E, c, t)
FOREACH node v ∈ V :
  d[v] ← ∞.
  successor[v] ← null.
d[t] ← 0.
FOR i = 1 TO n – 1
  FOREACH node w ∈ V :
    IF (d[w] was updated in previous pass)
      FOREACH edge (v, w) ∈ E :
        IF (d[v] > d[w] + ℓvw)
          d[v] ← d[w] + ℓvw.
          successor[v] ← w.
  IF (no d[⋅] value changed in pass i) STOP.
```
- 

### 음수 사이클 찾기

