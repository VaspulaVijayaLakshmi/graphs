# graphs


this repository also contains CSES problems tasks
https://cses.fi/problemset/task/1192


How to identify DSU?
Well, in questions where we are asked to reach one node from another, or where we need to attach nodes or form components of similiar types,
we go with DSU. In DSU questions, most of the time, we are not given any extra conditions on the path, like shortest, longest, etc. We just need to find if we could reach from one node to another.


Notes:

# Path With Minimum Effort

Was thinking this to be DP.  
But how can we solve this with PQ?



### Concern
What if the cell paths are min min min.. but boom at last we hit max height?  
We would've skipped the other path if they are greater in the beginning but lesser moving forward.  
We are not considering all possibilities with PQ, right?



### Why PQ / Dijkstra Still Works

The PQ stores **(current_path_effort, cell)** —  
`current_path_effort` = max of all edges so far along that path.

We do **NOT** mark a cell as “done” the first time we visit it like in normal Dijkstra on additive costs.  
Instead, we update the cell’s minimum known effort and push it back into the PQ if we find a better path.

So even if Path A gets to the cell first, and Path B comes later:  
- If Path B has smaller max effort, we update and push it again.  
- PQ ensures that paths with smaller max effort are explored first.  

This way, no “better future path” is skipped.


______________

Spanning Tree:

-> Prim’s Algorithm uses a Priority Queue (Min-Heap).

//put the 


-> Kruskal’s Algorithm does not — it uses sorting + Disjoint Set Union (DSU).

//sort the edges
//add it to DSU, if they belong, then skip, else add it to dsu.

________


// Comparator for sorting edges by weight
    static bool cmp(const vector<int>& a, const vector<int>& b) {
        return a[2] < b[2]; // sort by weight (ascending)
    }

________



### WHILE REVISING NOW GO TO GFG AND PRACTICE NEW QUESTION
##GIVE IT A TRY

##AT THE BOTTOM OF THE, gfg
https://www.geeksforgeeks.org/dsa/dijkstras-shortest-path-algorithm-greedy-algo-7/






________________



# Shortest Path with At Most One Special Edge

In some problems, we’re asked to find the minimum distance but can use **at most one special edge** — for example, at most one curved edge, or at most one stop in flight path problems.

---

## Understanding the Constraint

Normal Dijkstra finds the shortest path with no restriction.  
Here, the constraint is: **we can use at most 1 special edge (curved edge)** — you either use 0 or 1.

 **Key observation:** this introduces **state** — your “distance” now depends not only on the node but also on whether you’ve used the curved edge.

---

## Think in terms of Stateful Dijkstra

Instead of just `dist[node]`, maintain:

dist[node][used]


- `used = 0` → haven’t used a curved edge yet.  
- `used = 1` → already used a curved edge.

Now your Dijkstra (or BFS/priority queue) tracks the **minimum distance for each state**.

---

## Algorithm (Dijkstra-like)

1. Initialize  


dist[node][0] = 0 // for source
dist[node][1] = ∞


2. Use a **priority queue** storing `(current_cost, node, used)`.  
3. Pop `(cost, node, used)` from PQ.  
4. For each neighbor:  
- **Normal edge:**  
  - Can always take it, next state `used` stays the same.  
- **Curved edge:**  
  - Can take only if `used == 0`, next state becomes `used = 1`.  
5. If the new distance is smaller than `dist[next][next_used]`, update and push into PQ.  
6. Continue until the PQ is empty.

---

This way, you extend normal Dijkstra to work on two layers —  
one layer when the special edge is unused, and one when it’s already used.  
It guarantees that all valid paths (with or without the special edge) are explored efficiently, and the final answer is the minimum of both `dist[target][0]` and `dist[target][1]`.



IMPORTANT PROBLEM:
Shortest Path Using Atmost One Curved Edge
-> https://www.geeksforgeeks.org/problems/shortest-path-using-atmost-one-curved-edge--170647/1
-> https://cses.fi/problemset/task/1195

