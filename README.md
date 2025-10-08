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

# Shortest Path Using At Most One Curved Edge

**Problem Links:**  
- https://www.geeksforgeeks.org/problems/shortest-path-using-atmost-one-curved-edge--170647/1  
- https://cses.fi/problemset/task/1195



This way, you extend normal Dijkstra to work on two layers —  
one layer when the special edge is unused, and one when it’s already used.  

It guarantees that all valid paths (with or without the special edge) are explored efficiently,  
and the final answer is the minimum of both `dist[target][0]` and `dist[target][1]`.

**IMPORTANT PROBLEM:** Shortest Path Using Atmost One Curved Edge



_________



IN Dijstras- once the way is optimized its final.
Even if u find a new node which reaches to you in the same amount of diustance or time

we need not push it again to queue
Thats just increase the number of ways to reach.
https://www.geeksforgeeks.org/problems/number-of-ways-to-arrive-at-destination/1


Also when you are taking the cummulative sum
DOnt assign like 
if a new way is found then directly dont assign ways[nnode]=1 -> something

Cummulate the parent's sum

ways[child_ node]  += ways[parent_node] 


_______


Digjstra works on monotonous graphs.
That is  -ve or +ve edges
or graph with No negative edges.

If we are sure there wont be negative graph cycle then dont use Dikjsstra at all



__________


If we are asked about max time needed or somethings, and graph doesnt contains cycles
then we can negate(-) the edges.

___

Approach	           Handles Negatives	Detects Positive       Cycles	Works Here?
Dijkstra	                ❌	                    ❌	               ❌
Bellman-Ford	            ✅	                    ✅	               ✅
Topo DP (if DAG)	        ✅	                     N/A	        Only if DAG

____

When we negate all edge weights (w' = -w):

Original            Edge Weight	  After Negation	Effect on Cycle
Positive edge	Negative edge	Positive cycle → becomes negative cycle
Negative edge	Positive edge	Negative cycle → becomes positive cycle

_______


Bellman-Ford detects negative cycles (meaning: total weight keeps decreasing forever).
But we are finding maximum score — that’s like saying:

“Can we increase our score infinitely?”

So when we negate the weights:

Infinite increase (positive cycle)
→ becomes infinite decrease (negative cycle)
→ Bellman-Ford can detect it

______

Eg: https://cses.fi/problemset/task/1673
HIGH SCORE 


https://cses.fi/problemset/task/1680
Longest Flight Route


_______________
_______________



Topological Sort + DP Approach

Longest Flight Route

We will find the order of elements, then we will traverse the graph order wise.



Topological Sort + DP Approach
Idea:

Graph is a DAG → there exists at least one topological ordering of nodes.
We want longest path in terms of number of cities.
Use DP along topological order.


Topological order ensures: when we process a node, all its predecessors have been processed.
So dp[u] is already maximum possible cities up to node u.
No cycles → no need for relaxation multiple times.

_______

TOPO + DP works with the Weighted edges as well as long as their are no engative edges

```
1.

vector<long long> dp(n+1, LLONG_MIN); // for max path
dp[start] = 0;  // distance to start node
vector<int> parent(n+1, -1); // to reconstruct path


2.
Traverse nodes in topo order:

3.
for(int u : topo_order){
    if(dp[u] == LLONG_MIN) continue; // unreachable

    for(auto [v, w] : adj[u]){
        if(dp[v] < dp[u] + w){
            dp[v] = dp[u] + w;
            parent[v] = u;
        }
    }
}


