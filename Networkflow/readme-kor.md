# Lec10. networkflow

## Flow network

#### 흐름 네트워크(flow network)는 튜플 G = (V, E, s, t, c) 이다. 

- 소스(source) s ∈ V와 싱크(sink) t ∈ V가 존재하는 방향 그래프(Digraph) (V, E).
; 모든 노드가 s로부터 도달 가능한 것으로 가정함.
- 각 e ∈ E에 대해 용량 c(e) ≥ 0.
직관. 운송 네트워크를 통한 재료의 흐름; 재료는 소스에서 발생하여 싱크로 보내진다.

<img src="https://github.com/yshghid/Algorithm/assets/153489198/d48e3832-3eb9-412d-968b-36847a6b4ddf" alt="Flow Network" width="600">

## Minimum-cut problem

<image src="https://github.com/yshghid/Algorithm/assets/153489198/ac535761-d3e3-4475-9982-f2b1f991b8e2" width = "600">

그림에서, {s} = *A* 와 {1,2,3,4,5,6,t} = *B* 이고 이 st-cut의 용량은 다음과 같다.

- capacity = 10(s→1) + 5(s→3) + 15(s→5) = 30

<img src="https://github.com/yshghid/Algorithm/assets/153489198/abe1a54d-8cee-4fe3-974a-c0d76062bcb8" width = "600">

그림에서, {s, 3, 5, 6} = A 와 {1, 2, 4, t} = B 이고 이 st-cut의 용량은 다음과 같다.

- capacity = 10(s→1) + 8(3→4) + 10(6→t) = 28

<img src="https://github.com/yshghid/Algorithm/assets/153489198/daa372e3-8171-4081-9979-766c4efc5316" width = "600">

그림에서, 주어진 st-cut의 용량은?

**A.** 11 (20 + 25 − 8 − 11 − 9 − 6)

**B.** 34 (8 + 11 + 9 + 6)

**C.** 45 (20 + 25)

**D.** 79 (20 + 25 + 8 + 11 + 9 + 6)

- capacity = 20(s→1) + 25(6→t) = 45

## Maximum-flow problem

최대 흐름 문제는 소스 s에서 싱크 t로 가는 최대 값을 가진 흐름 f를 찾는 것이다.

#### 최대 흐름 문제 - 그리디 알고리즘

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

- 흐름 f(s->1): 8

<img src="https://github.com/yshghid/Algorithm/assets/153489198/aea266f9-03f9-4a5e-87b5-e329a6bbea5c" width=600>

4. 반복: 더 이상 증강 경로를 찾을 수 없을 때까지 증강 경로를 찾고 흐름을 증가시킨다.

- 두번째 증강 경로: s->1->3->4->t
- 경로를 따라 흐름이 2 증가: f(s->1) = 10, f(1->3) = 2, f(3->4)=2, f(4->t) = 10

- 흐름 f(s->1): 10

<img src="https://github.com/yshghid/Algorithm/assets/153489198/cddf49ad-52ed-414c-aaab-a5a49fd3c3ff" width=600>

- 세번째 증강 경로: s->3->4->2->t
- 경로를 따라 흐름이 6 증가: f(s->3) = 6, f(3->4) = 8, f(4->2) = 6, f(2->t) = 6

- 흐름 f(s->2): 6

<img src="https://github.com/yshghid/Algorithm/assets/153489198/ce3d0898-8e22-498f-aebd-87c112ed64d9" width=600>

5. 최종 흐름 값

- 소스 s에서 나가는 흐름의 합은 최종 흐름 값이다. 이 경우, val(f) = f(s->1) + f(s->2) = 10+6 = 16

6. 하지만, 주어진 그래프에서 s에서 t로 가는 최대 흐름은 19이다.

  <img src="https://github.com/yshghid/Algorithm/assets/153489198/32202c59-794c-4cc6-855c-727ad91724b7" width=600>

