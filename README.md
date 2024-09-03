# Dinic


Dinic's algorithm is a strongly polynomial algorithm used to compute the maximum flow in a flow network. It improves upon the Ford-Fulkerson method by using the concept of a level graph and finding blocking flows efficiently. This approach allows Dinic’s algorithm to achieve better performance, especially for networks with high capacities and many edges.


Flow Network:
A flow network is a directed graph G = (V, E) where:
V is the set of vertices.
E is the set of directed edges.
Each edge (u, v) in E has a capacity c(u, v) which represents the maximum amount of flow that can pass through the edge.
The flow f(u, v) on each edge must satisfy:
Capacity constraint: 0 <= f(u, v) <= c(u, v)
Flow conservation: For every vertex u in V (except the source s and sink t), the total flow into u equals the total flow out of u.

Level Graph:
A level graph is a transformation of the residual graph where vertices are assigned levels based on their shortest distance (in terms of number of edges) from the source s.
The level of the source s is 0, and for any vertex v, its level is one more than the level of the previous vertex if there is a path from s to v along which flow can be pushed.

Blocking Flow:
A blocking flow is a flow in the level graph such that no more flow can be pushed from the source s to the sink t without violating the level structure.
The blocking flow saturates at least one edge on every path from s to t.


Construct the Residual Graph:
The residual graph G_f is constructed by keeping track of the remaining capacities on each edge, where the residual capacity c_f(u, v) = c(u, v) - f(u, v).

BFS to Build Level Graph:
Perform a Breadth-First Search (BFS) from the source s to assign levels to each vertex.
The BFS stops when the sink t is reached or when all reachable vertices have been leveled.
If the sink t is not reachable, the algorithm terminates with the current maximum flow.

DFS to Find Blocking Flow:
Use Depth-First Search (DFS) on the level graph to find augmenting paths and push flow along these paths.
The DFS ensures that the flow is pushed along paths where each edge goes to a vertex of the next level (i.e., level(v) = level(u) + 1).

Repeat Until No More Flow Can Be Pushed:
Repeatedly build level graphs and find blocking flows until no more flow can be sent from s to t.
The sum of all blocking flows found during the iterations gives the maximum flow in the network.


Initialization:
Set initial flow f(u, v) = 0 for all edges (u, v) in E.

Residual Capacity:
The residual capacity c_f(u, v) is:
c_f(u, v) = c(u, v) - f(u, v)

Level Graph Construction (BFS):
Perform BFS from source s:
level(s) = 0
for each edge (u, v):
    if level(v) < 0 and c_f(u, v) > 0:
        level(v) = level(u) + 1
        
Blocking Flow (DFS):
Perform DFS on the level graph to find blocking flows:
dfs(u, flow):
    if u == t:
        return flow
    for each edge (u, v):
        if level(v) == level(u) + 1 and c_f(u, v) > 0:
            min_flow = min(flow, c_f(u, v))
            temp_flow = dfs(v, min_flow)
            if temp_flow > 0:
                f(u, v) += temp_flow
                f(v, u) -= temp_flow
                return temp_flow
    return 0
    
Max Flow Computation:
The maximum flow is computed by summing all the flows found during the DFS calls until no more blocking flows can be found:
while bfs():
    while (flow = dfs(s, inf)) > 0:
        max_flow += flow

        
Time Complexity
The time complexity of Dinic’s algorithm is O(V^2 * E) in general, but it improves to O(V^(2/3) * E) for unit capacity networks, making it efficient for large graphs.



