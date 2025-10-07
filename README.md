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
