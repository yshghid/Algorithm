# Lec10. networkflow

## Flow network

#### A flow network is a tuple G = (V, E, s, t, c).

- Digraph (V, E) with source s ∈ V and sink t ∈ V
; assume all nodes are reachable from s
- Capacity c(e) ≥ 0 for each e ∈ E.
- Intuition. Material flowing through a transportation network; material originates at source and is sent to sink.

<img src="https://github.com/yshghid/Algorithm/assets/153489198/d48e3832-3eb9-412d-968b-36847a6b4ddf" alt="Flow Network" width="600">


## Minimum-cut problem

#### Def

- Def. An st-cut (cut) is a partition (A, B) of the nodes with s ∈ A and t ∈ B.
- Def. Its capacity is the sum of the capacities of the edges from A to B.

<image src="https://github.com/yshghid/Algorithm/assets/153489198/ac535761-d3e3-4475-9982-f2b1f991b8e2" width = "600">

In the figure, {s} = A and {1,2,3,4,5,6,t} = B, and the capacity of this st-cut is as follows.

- capacity = 10(s→1) + 5(s→3) + 15(s→5) = 30

<img src="https://github.com/yshghid/Algorithm/assets/153489198/abe1a54d-8cee-4fe3-974a-c0d76062bcb8" width = "600">

In the figure, {s, 3, 5, 6} = A and {1, 2, 4, t} = B, and the capacity of this st-cut is as follows.

- capacity = 10(s→1) + 8(3→4) + 10(6→t) = 28

<img src="https://github.com/yshghid/Algorithm/assets/153489198/daa372e3-8171-4081-9979-766c4efc5316" width = "600">

In the figure, which is the capacity of the given *st*-cut?

**A.** 11 (20 + 25 − 8 − 11 − 9 − 6)

**B.** 34 (8 + 11 + 9 + 6)

**C.** 45 (20 + 25)

**D.** 79 (20 + 25 + 8 + 11 + 9 + 6)

- capacity = 20(s→1) + 25(6→t) = 45

## Maximum-flow problem

The maximum-flow problem aims to find a flow f of maximum value from the source s to the sink t.

#### Greedy algorithm

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

#### Why the greedy algorithm fails

Q. Why does the greedy algorithm fail?
A. Once greedy algorithm increases flow on an edge, it never decreases it.

Ex. Consider flow network G.
・ The unique max flow f * has f*(v, w) = 0.
・ Greedy algorithm could choose s→v→w→t as first path.
