# Lec10. networkflow

## Flow network

- G = (V, E, s, t, c).
- Digraph (V, E) with source s ∈ V and sink t ∈ V; assume all nodes are reachable from s
- Capacity c(e) ≥ 0 for each e ∈ E.
- Intuition. Material flowing through a transportation network; material originates at source and is sent to sink.

<img src="https://github.com/yshghid/Algorithm/assets/153489198/d48e3832-3eb9-412d-968b-36847a6b4ddf" alt="Flow Network" width="600">


## Minimum-cut problem

- An st-cut (cut) is a partition (A, B) of the nodes with s ∈ A and t ∈ B.
- Its capacity is the sum of the capacities of the edges from A to B.

<image src="https://github.com/yshghid/Algorithm/assets/153489198/ac535761-d3e3-4475-9982-f2b1f991b8e2" width = "600">

- In the figure, {s} = A and {1,2,3,4,5,6,t} = B
- The capacity of this st-cut: 10(s→1) + 5(s→3) + 15(s→5) = 30

<img src="https://github.com/yshghid/Algorithm/assets/153489198/abe1a54d-8cee-4fe3-974a-c0d76062bcb8" width = "600">

- In the figure, {s, 3, 5, 6} = A and {1, 2, 4, t} = B
- The capacity of this st-cut: 10(s→1) + 8(3→4) + 10(6→t) = 28

## Maximum-flow problem

The maximum-flow problem aims to find a flow f of maximum value from the source s to the sink t.

### Maximum-flow problem - Greedy algorithm

<img src="https://github.com/yshghid/Algorithm/assets/153489198/ccb9c791-78cb-4dda-af7d-ac62989401b0" width=600>

Find the maximum flow using a greedy algorithm.

1. Initial State: Start with f(e) = 0 for all e∈E.

2. First Augmenting Path: Find a path with positive residual capacity; Find an path P where each edge has f(e)<c(e).

- Path s->1->4->t
- Minimum residual capacity along the path: min(10, 8, 10) = 8
- Update flow along the path(+8): f(s->1) = 8, f(1->4) = 8, f(4->t) = 8

3. Update residual capacities

- Residual capacity of s->1 becomes 2 (10-8)
- Residual capacity of 1->4 becomes 0 (8-8)
- Residual capacity of 4->t becomes 2 (10-8)
- Flow value f(s->1): 8

<img src="https://github.com/yshghid/Algorithm/assets/153489198/aea266f9-03f9-4a5e-87b5-e329a6bbea5c" width=600>

4. Repeat: Continue finding and augmenting paths until no more augmenting paths can be found.

- Second Augmenting Path: s->1->3->4->t
- Update flow along the path(+2): f(s->1) = 10, f(1->3) = 2, f(3->4)=2, f(4->t) = 10
- Flow value f(s->1): 10

<img src="https://github.com/yshghid/Algorithm/assets/153489198/cddf49ad-52ed-414c-aaab-a5a49fd3c3ff" width=600>

- Third Augmenting Path: s->3->4->2->t
- Update flow along the path(+6): f(s->3) = 6, f(3->4) = 8, f(4->2) = 6, f(2->t) = 6
- Flow value f(s->2): 6

<img src="https://github.com/yshghid/Algorithm/assets/153489198/ce3d0898-8e22-498f-aebd-87c112ed64d9" width=600>

5. Final Flow Value

- The sum of the flows out of s is the maximum flow value, which in this case is: val(f) = f(s->1) + f(s->2) = 10+6 = 16

6. But max-flow value maximum flow from s to t in the given graph is 19.

  <img src="https://github.com/yshghid/Algorithm/assets/153489198/32202c59-794c-4cc6-855c-727ad91724b7" width=600>

### Why the greedy algorithm fails

Q. Why does the greedy algorithm fail?
A. Once greedy algorithm increases flow on an edge, it never decreases it.

<img src="https://github.com/yshghid/Algorithm/assets/153489198/f92d9a6d-5dab-4b87-99a2-652f145ddd67" width=600>

Ex. Consider flow network G.

- The greedy algorithm chose the path s→v→w→t as the first path because it finds any augmenting path with available capacity.
- If the greedy algorithm sends a flow of f(s→v)=1, f(v→w)=1, and f(w→t)=1 through the path s→v→w→t, the capacity of the edge v→w will be fully utilized, resulting in the unique max flow f*(v,w)=0.
- The correct maximum flow should use a different configuration that does not use the v \to wv→w edge.
- Because the greedy algorithm locks in flow on certain edges, it cannot find the optimal solution, thus a mechanism to "undo" bad decisions is necessary.

## Residual network

- In the flow network G, the original edge e has the property of flow f(e) (the current flow passing through the edge) and capacity c(e) (the maximum flow that the edge can handle).
- The residual network Gf includes a reverse edge e(reverse). The reverse edge allows for the "undoing" of the flow sent through the original edge e.
- The residual capacity of the original edge e in the residual network is cf(e) = c(e)-f(e), which represents the remaining capacity to increase the flow in the original direction.
- The residual capacity of the reverse edge e(reverse) in the residual network is cf(e(reverse)) = f(e), which represents the capacity to decrease the flow in the reverse direction.

<img src="https://github.com/yshghid/Algorithm/assets/153489198/913302ff-b39b-4575-990b-0fbfb7ca0855" width=300>

