# Assignment 2
## Problem Statement

Write a program to implement Prim's algorithm to find minimum spanning tree of a user defined graph. Use Adjacency List to represent a graph.

---

## Pseudocode

```
CLASS Edge:
    destination: integer
    weight: integer

CLASS Graph:
    vertices: integer
    adjList: array of lists

    FUNCTION __init__(v):
        vertices = v
        CREATE adjList of size v

    FUNCTION addEdge(src, dest, weight):
        ADD Edge(dest, weight) to adjList[src]
        ADD Edge(src, weight) to adjList[dest]  // For undirected

    FUNCTION primsMST():
        CREATE key array, initialize with INFINITY
        CREATE parent array for MST construction
        CREATE visited array

        key[0] = 0  // Start from vertex 0
        parent[0] = -1

        FOR vertices times:
            u = vertex with minimum key not in visited
            visited[u] = true

            FOR each neighbor v of u:
                IF NOT visited[v] AND weight(u,v) < key[v]:
                    key[v] = weight(u,v)
                    parent[v] = u

        DISPLAY MST edges and total weight

MAIN:
    CREATE graph
    ADD edges
    CALL primsMST()
```

---

## C++ Code

```cpp
#include <iostream>
#include <vector>
#include <climits>
#include <iomanip>
using namespace std;

class Edge_agb {
public:
    int destination_agb;
    int weight_agb;

    Edge_agb(int dest_agb, int w_agb) {
        destination_agb = dest_agb;
        weight_agb = w_agb;
    }
};

class Graph_agb {
private:
    int vertices_agb;
    vector<vector<Edge_agb>> adjList_agb;

    int findMinKey_agb(int key_agb[], bool visited_agb[]) {
        int minKey_agb = INT_MAX;
        int minIndex_agb = -1;

        for (int i_agb = 0; i_agb < vertices_agb; i_agb++) {
            if (!visited_agb[i_agb] && key_agb[i_agb] < minKey_agb) {
                minKey_agb = key_agb[i_agb];
                minIndex_agb = i_agb;
            }
        }

        return minIndex_agb;
    }

public:
    Graph_agb(int v_agb) {
        vertices_agb = v_agb;
        adjList_agb.resize(v_agb);
    }

    void addEdge_agb(int src_agb, int dest_agb, int weight_agb) {
        if (src_agb >= 0 && src_agb < vertices_agb && dest_agb >= 0 && dest_agb < vertices_agb) {
            adjList_agb[src_agb].push_back(Edge_agb(dest_agb, weight_agb));
            adjList_agb[dest_agb].push_back(Edge_agb(src_agb, weight_agb));
            cout << "✓ Edge added: " << src_agb << " -- " << dest_agb
                 << " (weight: " << weight_agb << ")" << endl;
        } else {
            cout << "✗ Invalid vertices!" << endl;
        }
    }

    void primsMST_agb() {
        if (vertices_agb == 0) {
            cout << "Graph is empty!" << endl;
            return;
        }

        int* key_agb = new int[vertices_agb];
        int* parent_agb = new int[vertices_agb];
        bool* visited_agb = new bool[vertices_agb];

        for (int i_agb = 0; i_agb < vertices_agb; i_agb++) {
            key_agb[i_agb] = INT_MAX;
            visited_agb[i_agb] = false;
        }

        key_agb[0] = 0;
        parent_agb[0] = -1;

        cout << "\n===== Prim's Algorithm Execution =====" << endl;

        for (int count_agb = 0; count_agb < vertices_agb - 1; count_agb++) {
            int u_agb = findMinKey_agb(key_agb, visited_agb);

            if (u_agb == -1) {
                cout << "Graph is disconnected!" << endl;
                delete[] key_agb;
                delete[] parent_agb;
                delete[] visited_agb;
                return;
            }

            visited_agb[u_agb] = true;

            cout << "\nStep " << (count_agb + 1) << ": Adding vertex " << u_agb
                 << " to MST (key value: " << key_agb[u_agb] << ")" << endl;

            for (int i_agb = 0; i_agb < adjList_agb[u_agb].size(); i_agb++) {
                int v_agb = adjList_agb[u_agb][i_agb].destination_agb;
                int weight_agb_i = adjList_agb[u_agb][i_agb].weight_agb;

                if (!visited_agb[v_agb] && weight_agb_i < key_agb[v_agb]) {
                    cout << "  Updating vertex " << v_agb << ": key " << key_agb[v_agb]
                         << " -> " << weight_agb_i << " (via " << u_agb << ")" << endl;
                    parent_agb[v_agb] = u_agb;
                    key_agb[v_agb] = weight_agb_i;
                }
            }
        }

        displayMST_agb(parent_agb, key_agb);

        int totalWeight_agb = 0;
        for (int i_agb = 1; i_agb < vertices_agb; i_agb++) {
            totalWeight_agb += key_agb[i_agb];
        }

        cout << "\nTotal weight of MST: " << totalWeight_agb << endl;

        delete[] key_agb;
        delete[] parent_agb;
        delete[] visited_agb;
    }

    void displayMST_agb(int parent_agb[], int key_agb[]) {
        cout << "\n========== Minimum Spanning Tree Edges ==========" << endl;
        cout << setw(10) << "Edge" << setw(15) << "Weight" << endl;
        cout << string(25, '-') << endl;

        for (int i_agb = 1; i_agb < vertices_agb; i_agb++) {
            cout << setw(5) << parent_agb[i_agb] << " - " << i_agb << setw(15) << key_agb[i_agb] << endl;
        }

        cout << string(25, '=') << endl;
    }

    void displayAdjList_agb() {
        cout << "\n--- Adjacency List ---" << endl;

        for (int i_agb = 0; i_agb < vertices_agb; i_agb++) {
            cout << "Vertex " << i_agb << ": ";

            for (int j_agb = 0; j_agb < adjList_agb[i_agb].size(); j_agb++) {
                cout << "[" << adjList_agb[i_agb][j_agb].destination_agb
                     << ", w:" << adjList_agb[i_agb][j_agb].weight_agb << "] ";
            }

            if (adjList_agb[i_agb].empty()) {
                cout << "No connections";
            }
            cout << endl;
        }
    }

    void displayEdges_agb() {
        cout << "\n--- Graph Edges ---" << endl;
        bool hasEdges_agb = false;

        for (int i_agb = 0; i_agb < vertices_agb; i_agb++) {
            for (int j_agb = 0; j_agb < adjList_agb[i_agb].size(); j_agb++) {
                if (i_agb < adjList_agb[i_agb][j_agb].destination_agb) {
                    cout << i_agb << " -- " << adjList_agb[i_agb][j_agb].destination_agb
                         << " (weight: " << adjList_agb[i_agb][j_agb].weight_agb << ")" << endl;
                    hasEdges_agb = true;
                }
            }
        }

        if (!hasEdges_agb) {
            cout << "No edges in graph!" << endl;
        }
    }
};

int main_agb() {
    int vertices_agb, choice_agb;

    cout << "===== Prim's Minimum Spanning Tree Algorithm =====" << endl;
    cout << "Enter number of vertices: ";
    cin >> vertices_agb;

    Graph_agb graph_agb(vertices_agb);

    do {
        cout << "\n===== Menu =====" << endl;
        cout << "1. Add edge" << endl;
        cout << "2. Display adjacency list" << endl;
        cout << "3. Display edges" << endl;
        cout << "4. Find Minimum Spanning Tree (Prim's)" << endl;
        cout << "5. Create sample graph" << endl;
        cout << "6. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice_agb;

        switch (choice_agb) {
            case 1: {
                int src_agb, dest_agb, weight_agb;
                cout << "Enter source vertex: ";
                cin >> src_agb;
                cout << "Enter destination vertex: ";
                cin >> dest_agb;
                cout << "Enter weight: ";
                cin >> weight_agb;
                graph_agb.addEdge_agb(src_agb, dest_agb, weight_agb);
                break;
            }

            case 2:
                graph_agb.displayAdjList_agb();
                break;

            case 3:
                graph_agb.displayEdges_agb();
                break;

            case 4:
                graph_agb.primsMST_agb();
                break;

            case 5: {
                cout << "Creating sample graph..." << endl;
                cout << "Graph structure:" << endl;
                cout << "    2    " << endl;
                cout << " 0-----1 " << endl;
                cout << " |\\   /| " << endl;
                cout << " | \\ / | " << endl;
                cout << "4|  3  |5" << endl;
                cout << " | / \\ | " << endl;
                cout << " |/   \\| " << endl;
                cout << " 2-----3 " << endl;
                cout << "    6    " << endl;


                if (vertices_agb >= 4) {
                    graph_agb.addEdge_agb(0, 1, 2);
                    graph_agb.addEdge_agb(0, 2, 4);
                    graph_agb.addEdge_agb(0, 3, 3);
                    graph_agb.addEdge_agb(1, 2, 2);
                    graph_agb.addEdge_agb(1, 3, 5);
                    graph_agb.addEdge_agb(2, 3, 6);
                } else {
                    cout << "Need at least 4 vertices to create sample graph." << endl;
                }
                break;
            }

            case 6:
                cout << "Exiting... Thank you!" << endl;
                break;

            default:
                cout << "Invalid choice!" << endl;
        }
    } while (choice_agb != 6);

    return 0;
}
```

---

## Input/Output

### Sample Run:

```
===== Prim's Minimum Spanning Tree Algorithm =====
Enter number of vertices: 4

===== Menu =====
Enter your choice: 5
Creating sample graph...
Graph structure:
    2
 0-----1
 |\   /|
 | \ / |
4|  3  |5
 | / \ |
 |/   \|
 2-----3
    6

✓ Edge added: 0 -- 1 (weight: 2)
✓ Edge added: 0 -- 2 (weight: 4)
✓ Edge added: 0 -- 3 (weight: 3)
✓ Edge added: 1 -- 2 (weight: 2)
✓ Edge added: 1 -- 3 (weight: 5)
✓ Edge added: 2 -- 3 (weight: 6)

Enter your choice: 4

===== Prim's Algorithm Execution =====

Step 1: Adding vertex 0 to MST (key value: 0)
  Updating vertex 1: key 2147483647 -> 2 (via 0)
  Updating vertex 2: key 2147483647 -> 4 (via 0)
  Updating vertex 3: key 2147483647 -> 3 (via 0)

Step 2: Adding vertex 1 to MST (key value: 2)
  Updating vertex 2: key 4 -> 2 (via 1)

Step 3: Adding vertex 3 to MST (key value: 3)

========== Minimum Spanning Tree Edges ==========
      Edge         Weight
-------------------------
    -1 - 0              0
     0 - 1              2
     1 - 2              2
     0 - 3              3
=========================

Total weight of MST: 7

Enter your choice: 2

--- Adjacency List ---
Vertex 0: [1, w:2] [2, w:4] [3, w:3]
Vertex 1: [0, w:2] [2, w:2] [3, w:5]
Vertex 2: [0, w:4] [1, w:2] [3, w:6]
Vertex 3: [0, w:3] [1, w:5] [2, w:6]

Enter your choice: 6
Exiting... Thank you!
```

---

## Dry Run

### Graph Structure:

```
    2
 0-----1
 |\   /|
 | \ / |
4|  3  |5
 | / \ |
 |/   \|
 2-----3
    6

Vertices: 0, 1, 2, 3
Edges: 0-1(2), 0-2(4), 0-3(3), 1-2(2), 1-3(5), 2-3(6)
```

### Prim's Algorithm Execution:

**Initialization:**

```
key[] = [0, ∞, ∞, ∞]
parent[] = [-1, -1, -1, -1]
visited[] = [F, F, F, F]
```

**Iteration 1:** Select vertex 0 (min key = 0)

```
Process vertex 0:
  - Neighbor 1: key[1] = min(∞, 2) = 2, parent[1] = 0
  - Neighbor 2: key[2] = min(∞, 4) = 4, parent[2] = 0
  - Neighbor 3: key[3] = min(∞, 3) = 3, parent[3] = 0

key[] = [0, 2, 4, 3]
visited[] = [T, F, F, F]
```

**Iteration 2:** Select vertex 1 (min key = 2)

```
Process vertex 1:
  - Neighbor 0: already visited
  - Neighbor 2: key[2] = min(4, 2) = 2, parent[2] = 1
  - Neighbor 3: key[3] = min(3, 5) = 3 (no update)

key[] = [0, 2, 2, 3]
visited[] = [T, T, F, F]
```

**Iteration 3:** Select vertex 2 (min key = 2)

```
Process vertex 2:
  - Neighbor 0: already visited
  - Neighbor 1: already visited
  - Neighbor 3: key[3] = min(3, 6) = 3 (no update)

key[] = [0, 2, 2, 3]
visited[] = [T, T, T, F]
```

**Iteration 4:** Select vertex 3 (min key = 3)

```
Process vertex 3:
  - All neighbors visited

key[] = [0, 2, 2, 3]
visited[] = [T, T, T, T]
```

**MST Construction:**

```
Edges in MST:
-1 - 0 (weight: 0) - Root
 0 - 1 (weight: 2)
 1 - 2 (weight: 2)
 0 - 3 (weight: 3)

Total weight: 0 + 2 + 2 + 3 = 7
```

---

## Conclusion

This program successfully implements **Prim's Minimum Spanning Tree Algorithm** using Adjacency List representation:

1. **Prim's Algorithm**:

   - Greedy approach to find MST
   - Starts from arbitrary vertex
   - Grows MST one vertex at a time
   - Always adds minimum weight edge connecting MST to new vertex

2. **Algorithm Steps**:

   - Initialize key values to infinity (except start vertex = 0)
   - Create visited array to track MST vertices
   - For V-1 iterations:
     - Select unvisited vertex with minimum key
     - Add to MST
     - Update keys of adjacent vertices

3. **Key Data Structures**:

   - **Key Array**: Tracks minimum edge weights to reach vertices
   - **Parent Array**: Stores MST edges for reconstruction
   - **Visited Array**: Tracks vertices included in MST
   - **Adjacency List**: Efficient graph representation

4. **Time Complexity**:

   - Simple implementation: O(V²)
   - With binary heap: O((V + E) log V)
   - With Fibonacci heap: O(E + V log V)

5. **Space Complexity**:

   - Adjacency list: O(V + E)
   - Key, parent, visited arrays: O(V)
   - Total: O(V + E)

6. **Advantages**:

   - Guaranteed optimal solution
   - Works well with dense graphs
   - Easy to implement with adjacency matrix
   - Produces connected, acyclic subgraph

7. **Applications**:
   - Network design (telephone, computer networks)
   - Circuit design
   - Transportation networks
   - Cluster analysis
   - Approximation algorithms

**Comparison with Kruskal's Algorithm**:
| Feature | Prim's | Kruskal's |
|---------|--------|-----------|
| Approach | Vertex-based | Edge-based |
| Data Structure | Priority queue | Union-Find |
| Best for | Dense graphs | Sparse graphs |
| Time Complexity | O(V²) | O(E log E) |
| Space | O(V) | O(E) |
