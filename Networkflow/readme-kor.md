# Lec10. networkflow

## Flow network

<img src="https://github.com/yshghid/Algorithm/assets/153489198/d48e3832-3eb9-412d-968b-36847a6b4ddf" alt="Flow Network" width="600">

- G = (V, E, s, t, c)
- 소스(source) s ∈ V와 싱크(sink) t ∈ V가 존재하는 방향 그래프 (V, E); 모든 노드가 s로부터 도달 가능한 것으로 가정함.
- e ∈ E 일때 용량 c(e) ≥ 0.
- 직관적 정의: 운송 네트워크를 통한 재료의 흐름. 재료는 소스에서 발생하여 싱크로 보내진다.

## Minimum-cut problem

- st-cut은 노드 집합 V를 두 부분으로 나누는 것이다. 하나의 부분 집합에는 노드 s가 포함되고, 다른 부분 집합에는 노드 t가 포함된다.
- cut의 용량은 A에서 B로 가는 에지들의 용량의 합이다.

<image src="https://github.com/yshghid/Algorithm/assets/153489198/ac535761-d3e3-4475-9982-f2b1f991b8e2" width = "600">

- 위 그림에서 {s} = *A*, {1,2,3,4,5,6,t} = *B*
- st-cut의 용량: 10(s→1) + 5(s→3) + 15(s→5) = 30

<img src="https://github.com/yshghid/Algorithm/assets/153489198/abe1a54d-8cee-4fe3-974a-c0d76062bcb8" width = "600">

- 위 그림에서 {s, 3, 5, 6} = A, {1, 2, 4, t} = B
- st-cut의 용량: 10(s→1) + 8(3→4) + 10(6→t) = 28

<img src="https://github.com/yshghid/Algorithm/assets/153489198/daa372e3-8171-4081-9979-766c4efc5316" width = "600">

- 그림에서, 주어진 st-cut의 용량은?

**A.** 11 (20 + 25 − 8 − 11 − 9 − 6)

**B.** 34 (8 + 11 + 9 + 6)

**C.** 45 (20 + 25)

**D.** 79 (20 + 25 + 8 + 11 + 9 + 6)

- capacity = 20(s→1) + 25(6→t) = 45

## Maximum-flow problem

최대 흐름 문제는 소스 s에서 싱크 t로 가는 최대 값을 가진 흐름 f를 찾는 것이다.

### 최대 흐름 문제 - 그리디 알고리즘

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

### 그리디 알고리즘이 실패하는 이유

Q: 그리디 알고리즘이 실패하는 이유는?

A: 한 번 에지에 흐름을 증가시키면 이를 다시 줄일 수 없기 때문. 이는 잘못된 경로를 선택했을 때 최적의 해결책을 찾을 수 없게 만든다. 

<img src="https://github.com/yshghid/Algorithm/assets/153489198/f92d9a6d-5dab-4b87-99a2-652f145ddd67" width=600>

예시 네트워크 흐름 그래프 G.

- 그리디 알고리즘은 가능한 용량이 있는 아무 증강 경로를 찾기 때문에, 첫 번째 경로로 s→v→w→t를 선택하였다.
- 그리디 알고리즘이 s->v->w->t 경로를 통해 f(s->v) = 1, f(v->w) = 1, f(w->t) = 1의 흐름을 보낸다면, v->w 에지의 용량이 모두 소모되어 유일한 최대 흐름 f*(v, w) = 0을 가진다.
- 올바른 최대 흐름은 v->w 에지를 사용하지 않는 다른 구성을 사용해야 함.
- 그리디 알고리즘이 특정 엣지에 흐름을 잠그면 최적의 해결책을 찾을 수 없게 되므로 잘못된 결정을 "되돌릴" 수 있는 메커니즘이 필요.

## Residual network

- 흐름 네트워크 G에서 원래 에지 e가 갖는 특성은 흐름 f(e) (해당 엣지를 통과하는 현재 흐름), 용량 c(e)(해당 엣지가 처리할 수 있는 최대 흐름)이다.
- 잔여 네트워크 Gf는 역 에지 e(reverse)를 갖는다. 역 에지는 원래 엣지 e를 통해 보낸 흐름을 "되돌리는" 것을 허용한다.
- 잔여 네트워크의 원래 에지 e의 잔여 용량 cf(e) = c(e) - f(e) 는 원래 방향으로 흐름을 증가시킬 수 있는 남은 용량을 나타낸다. 
- 잔여 네트워크의 역 에지 e의 잔여 용량 cf(e(reverse)) = f(e) 는 역 방향으로 흐름을 줄일 수 있는 용량을 나타낸다.

<img src="https://github.com/yshghid/Algorithm/assets/153489198/913302ff-b39b-4575-990b-0fbfb7ca0855" width=300>

