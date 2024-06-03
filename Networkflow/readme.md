# Lec10. networkflow

## Flow network

#### A flow network is a tuple G = (V, E, s, t, c).

- Digraph (V, E) with source s ∈ V and sink t ∈ V
; assume all nodes are reachable from s
- Capacity c(e) ≥ 0 for each e ∈ E.
- Intuition. Material flowing through a transportation network; material originates at source and is sent to sink.

#### 흐름 네트워크(flow network)는 튜플 G = (V, E, s, t, c) 이다. 

- 소스(source) s ∈ V와 싱크(sink) t ∈ V가 존재하는 방향 그래프(Digraph) (V, E).
; 모든 노드가 s로부터 도달 가능한 것으로 가정함.
- 각 e ∈ E에 대해 용량 c(e) ≥ 0.
직관. 운송 네트워크를 통한 재료의 흐름; 재료는 소스에서 발생하여 싱크로 보내진다.

<img src="https://github.com/yshghid/Algorithm/assets/153489198/d48e3832-3eb9-412d-968b-36847a6b4ddf" alt="Flow Network" width="600">


## Minimum-cut problem

#### Def

- Def. An st-cut (cut) is a partition (A, B) of the nodes with s ∈ A and t ∈ B.
- Def. Its capacity is the sum of the capacities of the edges from A to B.

#### 정의

- 정의: st-절단(절단)은 노드를 s ∈ A와 t ∈ B로 나누는 것입니다.
- 정의: st-절단의 용량은 A에서 B로 이어지는 에지들의 용량의 합입니다.

<image src="https://github.com/yshghid/Algorithm/assets/153489198/ac535761-d3e3-4475-9982-f2b1f991b8e2" width = "600">

그림에서, {s} = *A* 와 {1,2,3,4,5,6,t} = *B* 이고 이 st-cut의 용량은 다음과 같다.

- capacity = 10(s→1) + 5(s→3) + 15(s→5) = 30

<img src="https://github.com/yshghid/Algorithm/assets/153489198/abe1a54d-8cee-4fe3-974a-c0d76062bcb8" width = "600">

그림에서, {s, 3, 5, 6} = A 와 {1, 2, 4, t} = B 이고 이 st-cut의 용량은 다음과 같다.

- capacity = 10(s→1) + 8(3→4) + 10(6→t) = 28

#### Nextflow - quiz 1

<img src="https://github.com/yshghid/Algorithm/assets/153489198/daa372e3-8171-4081-9979-766c4efc5316" width = "600">

Which is the capacity of the given *st*-cut?

**A.** 11 (20 + 25 − 8 − 11 − 9 − 6)

**B.** 34 (8 + 11 + 9 + 6)

**C.** 45 (20 + 25)

**D.** 79 (20 + 25 + 8 + 11 + 9 + 6)

- capacity = 20(s→1) + 25(6→t) = 45

주어진 st-cut의 용량은?

**A.** 11 (20 + 25 − 8 − 11 − 9 − 6)

**B.** 34 (8 + 11 + 9 + 6)

**C.** 45 (20 + 25)

**D.** 79 (20 + 25 + 8 + 11 + 9 + 6)

- capacity = 20(s→1) + 25(6→t) = 45

## Maximum-flow problem

#### Def

- Def: An st-flow(flow) f in a network is defined as a function that assigns a flow value to each edge, adhering to the following conditions:

1) Capacity Constraint: For each edge e∈E, the flow f(e) must be less than or equal to the capacity c(e) of that edge.

2) Flow Conservation: For each node v∈V - {st} , the sum of the flows into v must equal the sum of the flows out of v. This is also known as the conservation of flow.

- The value of a flow f is the total flow leaving the source s, which is the same as the total flow entering the sink t.

#### 정의

- 정의: st-흐름(흐름) f는 네트워크 내의 각 에지에 흐름 값을 할당하는 함수로, 다음 조건을 만족한다:

1) 용량 제약: 각 에지 e∈E 에 대해 흐름 f(e) 는 해당 에지의 용량 c(e) 보다 작거나 같아야 한다.

2) 흐름 보존: 소스 노드 s와 싱크 노드 t를 제외한 모든 노드 v∈V - {st} 에 대해, v로 들어오는 흐름의 합은 v에서 나가는 흐름의 합과 같아야 한다.

- 정의: 흐름의 값 f는 소스 s에서 나가는 총 흐름으로, 이는 싱크 t로 들어오는 총 흐름과 동일하다.

#### Maximum-flow problem

The maximum-flow problem aims to find a flow f of maximum value from the source s to the sink t.

#### 최대 흐름 문제

최대 흐름 문제는 소스 s에서 싱크 t로 가는 최대 값을 가진 흐름 f를 찾는 것이다.

<img src="https://github.com/yshghid/Algorithm/assets/153489198/ccb9c791-78cb-4dda-af7d-ac62989401b0" width=600>

그림에서 최대 흐름을 그리디 알고리즘으로 찾기. 

1. 초기 상태: 모든 e∈E 에 대해 f(e)는 0으로 시작한다.

2. 첫번째 증강 경로: 양수 잔여 용량이 있는 경로를 찾는다. 즉 각 에지에서 f(e)<c(e) 인 경로를 찾는다.

- 경로 s->1->4->t

- 경로의 최소 잔여 용량: min(10, 8, 10) = 8

- 경로를 따라 흐름이 8 증가: f(s->1) = 8, f(1->4) = 8, f(4->t) = 8

3. 잔여 용량 업데이트

- s->1의 잔여 용량은 2(10-8)
- 1->4의 잔여 용량은 0(8-8)
- 4->t의 잔여 용량은 2(10-8)

<img src="https://github.com/yshghid/Algorithm/assets/153489198/aea266f9-03f9-4a5e-87b5-e329a6bbea5c" width=600>

4. 반복: 더 이상 증강 경로를 찾을 수 없을 때까지 증강 경로를 찾고 흐름을 증가시킨다.

- 두번째 증강 경로: s->1->3->4->t
- 경로를 따라 흐름이 2 증가: f(s->1) = 10, f(1->3) = 2, f(3->4)=2, f(4->t) = 10



