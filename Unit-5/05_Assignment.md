# Assignment 5

## Problem Statement

Write a program to accept a graph from a user and represent it with Adjacency List and perform BFS and DFS traversals on it.

---

## Pseudocode

```
CLASS Graph:
    vertices: integer
    adjList: array of lists

    FUNCTION __init__(v):
        vertices = v
        CREATE adjList of size v

    FUNCTION addEdge(src, dest):
        ADD dest to adjList[src]
        ADD src to adjList[dest]  // For undirected graph

    FUNCTION BFS(startVertex):
        CREATE visited array initialized to false
        CREATE queue

        visited[startVertex] = true
        queue.enqueue(startVertex)

        WHILE queue is not empty:
            vertex = queue.dequeue()
            PRINT vertex

            FOR each neighbor of vertex:
                IF NOT visited[neighbor]:
                    visited[neighbor] = true
                    queue.enqueue(neighbor)

    FUNCTION DFS(startVertex):
        CREATE visited array initialized to false
        DFSRecursive(startVertex, visited)

    FUNCTION DFSRecursive(vertex, visited):
        visited[vertex] = true
        PRINT vertex

        FOR each neighbor of vertex:
            IF NOT visited[neighbor]:
                DFSRecursive(neighbor, visited)

    FUNCTION display():
        FOR each vertex:
            PRINT vertex and its neighbors

MAIN:
    INPUT number of vertices
    CREATE Graph object
    INPUT edges
    DISPLAY adjacency list
    PERFORM BFS
    PERFORM DFS
```

---

## C++ Code

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <iomanip>
#include <algorithm> // For sorting neighbors in DFS/BFS if required, though not strictly needed for correctness
using namespace std;

class Graph_agb {
private:
    int vertices_agb;
    vector<vector<int>> adjList_agb;

    // DFS recursive helper
    void DFSRecursive_agb(int vertex_agb, bool visited_agb[]) {
        visited_agb[vertex_agb] = true;
        cout << vertex_agb << " ";

        // Visit all adjacent vertices
        for (int i_agb = 0; i_agb < adjList_agb[vertex_agb].size(); i_agb++) {
            int neighbor_agb = adjList_agb[vertex_agb][i_agb];
            if (!visited_agb[neighbor_agb]) {
                DFSRecursive_agb(neighbor_agb, visited_agb);
            }
        }
    }

public:
    Graph_agb(int v_agb) {
        vertices_agb = v_agb;
        adjList_agb.resize(v_agb);
    }

    // Add edge to graph (undirected)
    void addEdge_agb(int src_agb, int dest_agb) {
        if (src_agb >= 0 && src_agb < vertices_agb && dest_agb >= 0 && dest_agb < vertices_agb) {
            adjList_agb[src_agb].push_back(dest_agb);
            adjList_agb[dest_agb].push_back(src_agb);  // For undirected graph
            cout << "✓ Edge added: " << src_agb << " -- " << dest_agb << endl;
        } else {
            cout << "✗ Invalid vertices!" << endl;
        }
    }

    // Add directed edge
    void addDirectedEdge_agb(int src_agb, int dest_agb) {
        if (src_agb >= 0 && src_agb < vertices_agb && dest_agb >= 0 && dest_agb < vertices_agb) {
            adjList_agb[src_agb].push_back(dest_agb);
            cout << "✓ Directed edge added: " << src_agb << " -> " << dest_agb << endl;
        } else {
            cout << "✗ Invalid vertices!" << endl;
        }
    }

    // BFS Traversal
    void BFS_agb(int startVertex_agb) {
        if (startVertex_agb < 0 || startVertex_agb >= vertices_agb) {
            cout << "Invalid start vertex!" << endl;
            return;
        }

        bool* visited_agb = new bool[vertices_agb];
        for (int i_agb = 0; i_agb < vertices_agb; i_agb++) {
            visited_agb[i_agb] = false;
        }

        queue<int> q_agb;

        cout << "\n--- BFS Traversal starting from vertex " << startVertex_agb << " ---" << endl;
        
        cout << "Order: ";

        visited_agb[startVertex_agb] = true;
        q_agb.push(startVertex_agb);

        while (!q_agb.empty()) {
            int vertex_agb = q_agb.front();
            q_agb.pop();
            cout << vertex_agb << " ";

            // Enqueue all adjacent unvisited vertices
            for (int i_agb = 0; i_agb < adjList_agb[vertex_agb].size(); i_agb++) {
                int neighbor_agb = adjList_agb[vertex_agb][i_agb];
                if (!visited_agb[neighbor_agb]) {
                    visited_agb[neighbor_agb] = true;
                    q_agb.push(neighbor_agb);
                }
            }
        }
        cout << endl;

        delete[] visited_agb;
    }

    // DFS Traversal
    void DFS_agb(int startVertex_agb) {
        if (startVertex_agb < 0 || startVertex_agb >= vertices_agb) {
            cout << "Invalid start vertex!" << endl;
            return;
        }

        bool* visited_agb = new bool[vertices_agb];
        for (int i_agb = 0; i_agb < vertices_agb; i_agb++) {
            visited_agb[i_agb] = false;
        }

        cout << "\n--- DFS Traversal starting from vertex " << startVertex_agb << " ---" << endl;
        
        cout << "Order: ";

        DFSRecursive_agb(startVertex_agb, visited_agb);
        cout << endl;

        delete[] visited_agb;
    }

    // Display adjacency list
    void displayAdjList_agb() {
        cout << "\n--- Adjacency List ---" << endl;

        for (int i_agb = 0; i_agb < vertices_agb; i_agb++) {
            cout << "Vertex " << i_agb << ": ";

            for (int j_agb = 0; j_agb < adjList_agb[i_agb].size(); j_agb++) {
                cout << adjList_agb[i_agb][j_agb] << " ";
            }

            if (adjList_agb[i_agb].empty()) {
                cout << "No connections";
            }
            cout << endl;
        }
    }

    // Display graph edges
    void displayEdges_agb() {
        cout << "\n--- Graph Edges ---" << endl;
        bool hasEdges_agb = false;

        for (int i_agb = 0; i_agb < vertices_agb; i_agb++) {
            for (int j_agb = 0; j_agb < adjList_agb[i_agb].size(); j_agb++) {
                // Only display each edge once (since it's undirected)
                if (i_agb < adjList_agb[i_agb][j_agb]) {
                    cout << i_agb << " -- " << adjList_agb[i_agb][j_agb] << endl;
                    hasEdges_agb = true;
                }
            }
        }

        if (!hasEdges_agb) {
            cout << "No edges in graph!" << endl;
        }
    }

    // Check if graph is connected
    bool isConnected_agb() {
        if (vertices_agb == 0) return true;
        if (vertices_agb == 1) return true;

        bool* visited_agb = new bool[vertices_agb];
        for (int i_agb = 0; i_agb < vertices_agb; i_agb++) {
            visited_agb[i_agb] = false;
        }

        // Find a starting vertex that has edges (important for disconnected graphs with isolated vertices)
        int startVertex_agb = -1;
        for (int i_agb = 0; i_agb < vertices_agb; i_agb++) {
            if (!adjList_agb[i_agb].empty() || vertices_agb == 1) { // Choose the first non-empty list or vertex 0 if only 1 vertex
                startVertex_agb = i_agb;
                break;
            }
        }

        if (startVertex_agb == -1 && vertices_agb > 0) {
             // Graph has vertices but no edges (all vertices are isolated). It's disconnected if vertices > 1.
             delete[] visited_agb;
             return vertices_agb == 1;
        }
        
        if (startVertex_agb == -1 && vertices_agb == 0) {
             delete[] visited_agb;
             return true; // Empty graph is considered connected.
        }

        // Start DFS from the found vertex
        DFSRecursiveForConnectivity_agb(startVertex_agb, visited_agb);

        // Check if all vertices were visited (must account for isolated vertices)
        bool connected_agb = true;
        for (int i_agb = 0; i_agb < vertices_agb; i_agb++) {
             // If a vertex has edges, it must be visited to be connected
             // If a vertex has no edges, it's an isolated vertex. The graph is disconnected if there's more than one vertex.
            if (!visited_agb[i_agb] && (!adjList_agb[i_agb].empty() || adjList_agb[i_agb].empty() && vertices_agb > 1)) {
                connected_agb = false;
                break;
            }
        }

        delete[] visited_agb;
        return connected_agb;
    }

    // DFS helper for connectivity check
    void DFSRecursiveForConnectivity_agb(int vertex_agb, bool visited_agb[]) {
        visited_agb[vertex_agb] = true;

        for (int i_agb = 0; i_agb < adjList_agb[vertex_agb].size(); i_agb++) {
            int neighbor_agb = adjList_agb[vertex_agb][i_agb];
            if (!visited_agb[neighbor_agb]) {
                DFSRecursiveForConnectivity_agb(neighbor_agb, visited_agb);
            }
        }
    }

    // Get degree of a vertex
    int getDegree_agb(int vertex_agb) {
        if (vertex_agb < 0 || vertex_agb >= vertices_agb) {
            cout << "Invalid vertex!" << endl;
            return -1;
        }
        return adjList_agb[vertex_agb].size();
    }
};

int main_agb() {
    int vertices_agb, choice_agb;

    cout << "===== Graph Traversal (BFS & DFS) using Adjacency List =====" << endl;
    cout << "Enter number of vertices: ";
    cin >> vertices_agb;

    Graph_agb graph_agb(vertices_agb);

    do {
        cout << "\n===== Menu =====" << endl;
        cout << "1. Add undirected edge" << endl;
        cout << "2. Add directed edge" << endl;
        cout << "3. Display adjacency list" << endl;
        cout << "4. Display edges" << endl;
        cout << "5. BFS Traversal" << endl;
        cout << "6. DFS Traversal" << endl;
        cout << "7. Check if graph is connected" << endl;
        cout << "8. Get vertex degree" << endl;
        cout << "9. Create sample graph" << endl;
        cout << "10. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice_agb;

        switch (choice_agb) {
            case 1: {
                int src_agb, dest_agb;
                cout << "Enter source vertex: ";
                cin >> src_agb;
                cout << "Enter destination vertex: ";
                cin >> dest_agb;
                graph_agb.addEdge_agb(src_agb, dest_agb);
                break;
            }

            case 2: {
                int src_agb, dest_agb;
                cout << "Enter source vertex: ";
                cin >> src_agb;
                cout << "Enter destination vertex: ";
                cin >> dest_agb;
                graph_agb.addDirectedEdge_agb(src_agb, dest_agb);
                break;
            }

            case 3:
                graph_agb.displayAdjList_agb();
                break;

            case 4:
                graph_agb.displayEdges_agb();
                break;

            case 5: {
                int start_agb;
                cout << "Enter starting vertex for BFS: ";
                cin >> start_agb;
                graph_agb.BFS_agb(start_agb);
                break;
            }

            case 6: {
                int start_agb;
                cout << "Enter starting vertex for DFS: ";
                cin >> start_agb;
                graph_agb.DFS_agb(start_agb);
                break;
            }

            case 7:
                if (graph_agb.isConnected_agb()) {
                    cout << "✓ Graph is **CONNECTED**" << endl;
                } else {
                    cout << "✗ Graph is **DISCONNECTED**" << endl;
                }
                break;

            case 8: {
                int vertex_agb;
                cout << "Enter vertex to get degree: ";
                cin >> vertex_agb;
                int degree_agb = graph_agb.getDegree_agb(vertex_agb);
                if (degree_agb != -1) {
                    cout << "Degree of vertex " << vertex_agb << ": " << degree_agb << endl;
                }
                break;
            }

            case 9: {
                cout << "Creating sample graph..." << endl;
                cout << "Graph structure (6 vertices: 0-5):" << endl;
                cout << "  0 -- 1 -- 2" << endl;
                cout << "  |    |    |" << endl;
                cout << "  3 -- 4 -- 5" << endl;

                if (vertices_agb >= 6) {
                    graph_agb.addEdge_agb(0, 1);
                    graph_agb.addEdge_agb(0, 3);
                    graph_agb.addEdge_agb(1, 2);
                    graph_agb.addEdge_agb(1, 4);
                    graph_agb.addEdge_agb(2, 5);
                    graph_agb.addEdge_agb(3, 4);
                    graph_agb.addEdge_agb(4, 5);
                } else {
                    cout << "Need at least 6 vertices to create sample graph." << endl;
                }
                break;
            }

            case 10:
                cout << "Exiting... Thank you!" << endl;
                break;

            default:
                cout << "Invalid choice!" << endl;
        }
    } while (choice_agb != 10);

    return 0;
}
```

---

## Input/Output

### Sample Run:

```
===== Graph Traversal (BFS & DFS) using Adjacency List =====
Enter number of vertices: 6

===== Menu =====
Enter your choice: 9
Creating sample graph...
Graph structure:
  0 -- 1 -- 2
  |    |    |
  3 -- 4 -- 5
✓ Edge added: 0 -- 1
✓ Edge added: 0 -- 3
✓ Edge added: 1 -- 2
✓ Edge added: 1 -- 4
✓ Edge added: 2 -- 5
✓ Edge added: 3 -- 4
✓ Edge added: 4 -- 5

Enter your choice: 3

--- Adjacency List ---
Vertex 0: 1 3
Vertex 1: 0 2 4
Vertex 2: 1 5
Vertex 3: 0 4
Vertex 4: 1 3 5
Vertex 5: 2 4

Enter your choice: 5
Enter starting vertex for BFS: 0

--- BFS Traversal starting from vertex 0 ---
Order: 0 1 3 2 4 5

Enter your choice: 6
Enter starting vertex for DFS: 0

--- DFS Traversal starting from vertex 0 ---
Order: 0 1 2 5 4 3

Enter your choice: 7
✓ Graph is CONNECTED

Enter your choice: 8
Enter vertex to get degree: 4
Degree of vertex 4: 3

Enter your choice: 4

--- Graph Edges ---
0 -- 1
0 -- 3
1 -- 2
1 -- 4
2 -- 5
3 -- 4
4 -- 5

Enter your choice: 10
Exiting... Thank you!
```

---

## Dry Run

### Graph Structure:

```
  0 -- 1 -- 2
  |    |    |
  3 -- 4 -- 5
```

### Adjacency List Representation:

```
Vertex 0: [1, 3]
Vertex 1: [0, 2, 4]
Vertex 2: [1, 5]
Vertex 3: [0, 4]
Vertex 4: [1, 3, 5]
Vertex 5: [2, 4]
```

### BFS Traversal from vertex 0:

**Initial State:**

- visited = [F, F, F, F, F, F]
- queue = []

**Step 1:** Start with vertex 0

- Mark 0 as visited: [T, F, F, F, F, F]
- Enqueue 0: queue = [0]

**Step 2:** Dequeue 0, process neighbors (1, 3)

- Output: 0
- Visit 1 and 3: visited = [T, T, F, T, F, F]
- queue = [1, 3]

**Step 3:** Dequeue 1, process neighbors (0, 2, 4)

- Output: 0 1
- 0 already visited, visit 2 and 4
- visited = [T, T, T, T, T, F]
- queue = [3, 2, 4]

**Step 4:** Dequeue 3, process neighbors (0, 4)

- Output: 0 1 3
- Both already visited
- queue = [2, 4]

**Step 5:** Dequeue 2, process neighbors (1, 5)

- Output: 0 1 3 2
- 1 already visited, visit 5
- visited = [T, T, T, T, T, T]
- queue = [4, 5]

**Step 6:** Dequeue 4, all neighbors visited

- Output: 0 1 3 2 4
- queue = [5]

**Step 7:** Dequeue 5, all neighbors visited

- Output: 0 1 3 2 4 5
- queue = []

**BFS Result:** 0 1 3 2 4 5

---

### DFS Traversal from vertex 0:

**Call Stack Visualization:**

```
DFS(0): visited[0] = T, output: 0
├─ DFS(1): visited[1] = T, output: 1
│  ├─ DFS(2): visited[2] = T, output: 2
│  │  ├─ DFS(5): visited[5] = T, output: 5
│  │  │  └─ (all neighbors visited)
│  │  └─ (returns)
│  ├─ DFS(4): visited[4] = T, output: 4
│  │  ├─ DFS(3): visited[3] = T, output: 3
│  │  │  └─ (all neighbors visited)
│  │  └─ (returns)
│  └─ (returns)
└─ (returns)
```

**Step-by-Step:**

1. Visit 0, mark visited → Output: 0
2. Go to first neighbor 1 → Output: 0 1
3. From 1, go to first unvisited neighbor 2 → Output: 0 1 2
4. From 2, go to first unvisited neighbor 5 → Output: 0 1 2 5
5. From 5, all neighbors visited, backtrack
6. From 2, all neighbors visited, backtrack
7. From 1, go to next unvisited neighbor 4 → Output: 0 1 2 5 4
8. From 4, go to first unvisited neighbor 3 → Output: 0 1 2 5 4 3
9. From 3, all neighbors visited, backtrack

**DFS Result:** 0 1 2 5 4 3

---

## Conclusion

This program successfully implements **Graph Traversal using Adjacency List** with the following achievements:

1. **Adjacency List Representation**:

   - Dynamic array of vectors
   - Space efficient for sparse graphs
   - O(V + E) space complexity
   - Easy to add/remove edges

2. **BFS (Breadth-First Search)**:

   - Uses queue data structure
   - Explores level by level
   - Shortest path in unweighted graphs
   - Time complexity: O(V + E)

3. **DFS (Depth-First Search)**:

   - Uses recursion (implicit stack)
   - Explores as far as possible before backtracking
   - Used for cycle detection, topological sort
   - Time complexity: O(V + E)

4. **Key Features**:

   - Support for both directed and undirected edges
   - Multiple traversal options
   - Graph connectivity checking
   - Vertex degree calculation
   - Edge listing and adjacency display

5. **Time Complexity**:

   - Add edge: O(1)
   - BFS: O(V + E)
   - DFS: O(V + E)
   - Display: O(V + E)

6. **Space Complexity**:
   - Adjacency list: O(V + E)
   - Visited array: O(V)
   - BFS queue: O(V)
   - DFS stack: O(V)

**Adjacency List vs Adjacency Matrix**:
| Feature | Adjacency List | Adjacency Matrix |
|---------|----------------|------------------|
| Space | O(V + E) | O(V²) |
| Add Edge | O(1) | O(1) |
| Check Edge | O(degree) | O(1) |
| List Neighbors | O(degree) | O(V) |
| Best for | Sparse graphs | Dense graphs |

**Applications**:

- Social network analysis
- Web crawling
- Network routing
- Game AI pathfinding
- Dependency resolution
- Garbage collection

**Traversal Differences**:
| Feature | BFS | DFS |
|---------|-----|-----|
| Data Structure | Queue | Stack |
| Order | Level-wise | Depth-wise |
| Memory | More (queue) | Less (recursion) |
| Path | Shortest in unweighted | Any path |
| Use Cases | Shortest path, spanning tree | Topological sort, cycle detection |
