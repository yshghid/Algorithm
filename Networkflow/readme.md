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

![image](https://github.com/yshghid/Algorithm/assets/153489198/d48e3832-3eb9-412d-968b-36847a6b4ddf)

## Minimum-cut problem

#### Def. An st-cut (cut) is a partition (A, B) of the nodes with s ∈ A and t ∈ B.
#### Def. Its capacity is the sum of the capacities of the edges from A to B.

#### 정의: st-절단(절단)은 노드를 s ∈ A와 t ∈ B로 나누는 것입니다.
#### 정의: st-절단의 용량은 A에서 B로 이어지는 에지들의 용량의 합입니다.

![image](https://github.com/yshghid/Algorithm/assets/153489198/ac535761-d3e3-4475-9982-f2b1f991b8e2)

그림에서, {s} = *A* 와 {1,2,3,4,5,6,t} = *B* 이고 이 st-cut의 용량은 다음과 같다.

- capacity = 10(s→1) + 5(s→3) + 15(s→5) = 30

![image](https://github.com/yshghid/Algorithm/assets/153489198/abe1a54d-8cee-4fe3-974a-c0d76062bcb8)

그림에서, {s, 3, 5, 6} = A 와 {1, 2, 4, t} = B 이고 이 st-cut의 용량은 다음과 같다.

- capacity = 10(s→1) + 8(3→4) + 10(6→t) = 28

#### Nextflow - quiz 1

![image](https://github.com/yshghid/Algorithm/assets/153489198/daa372e3-8171-4081-9979-766c4efc5316)

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


