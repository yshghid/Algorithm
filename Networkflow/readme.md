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
