
# Assignment 1

## Problem Statement

Write a program to accept a graph from a user and represent it with an Adjacency Matrix. Perform BFS (Breadth-First Search) and DFS (Depth-First Search) traversals on it.

---

## Pseudocode

```
CLASS Graph:
    vertices: integer
    adjMatrix[MAX][MAX]: 2D array

    FUNCTION __init__(v):
        vertices = v
        Initialize adjMatrix with 0s

    FUNCTION addEdge(src, dest):
        adjMatrix[src][dest] = 1
        adjMatrix[dest][src] = 1  // For undirected graph

    FUNCTION BFS(startVertex):
        CREATE visited array initialized to false
        CREATE queue

        visited[startVertex] = true
        queue.enqueue(startVertex)

        WHILE queue is not empty:
            vertex = queue.dequeue()
            PRINT vertex

            FOR each adjacent vertex i of vertex:
                IF adjMatrix[vertex][i] == 1 AND NOT visited[i]:
                    visited[i] = true
                    queue.enqueue(i)

    FUNCTION DFS(startVertex):
        CREATE visited array initialized to false
        DFSRecursive(startVertex, visited)

    FUNCTION DFSRecursive(vertex, visited):
        visited[vertex] = true
        PRINT vertex

        FOR each adjacent vertex i of vertex:
            IF adjMatrix[vertex][i] == 1 AND NOT visited[i]:
                DFSRecursive(i, visited)

    FUNCTION display():
        PRINT adjacency matrix

MAIN:
    INPUT number of vertices
    CREATE Graph object
    INPUT edges
    DISPLAY adjacency matrix
    PERFORM BFS
    PERFORM DFS
```

---

## C++ Code

```cpp
#include <iostream>
#include <queue>
#include <iomanip>
using namespace std;

#define MAX_agb 20

class Graph_agb {
private:
    int vertices_agb;
    int adjMatrix_agb[MAX_agb][MAX_agb];

    void DFSRecursive_agb(int vertex_agb, bool visited_agb[]) {
        visited_agb[vertex_agb] = true;
        cout << vertex_agb << " ";

        for (int i_agb = 0; i_agb < vertices_agb; i_agb++) {
            if (adjMatrix_agb[vertex_agb][i_agb] == 1 && !visited_agb[i_agb]) {
                DFSRecursive_agb(i_agb, visited_agb);
            }
        }
    }

public:
    Graph_agb(int v_agb) {
        vertices_agb = v_agb;

        for (int i_agb = 0; i_agb < vertices_agb; i_agb++) {
            for (int j_agb = 0; j_agb < vertices_agb; j_agb++) {
                adjMatrix_agb[i_agb][j_agb] = 0;
            }
        }
    }

    void addEdge_agb(int src_agb, int dest_agb) {
        if (src_agb >= 0 && src_agb < vertices_agb && dest_agb >= 0 && dest_agb < vertices_agb) {
            adjMatrix_agb[src_agb][dest_agb] = 1;
            adjMatrix_agb[dest_agb][src_agb] = 1;
            cout << "✓ Edge added: " << src_agb << " -- " << dest_agb << endl;
        } else {
            cout << "✗ Invalid vertices!" << endl;
        }
    }

    void addDirectedEdge_agb(int src_agb, int dest_agb) {
        if (src_agb >= 0 && src_agb < vertices_agb && dest_agb >= 0 && dest_agb < vertices_agb) {
            adjMatrix_agb[src_agb][dest_agb] = 1;
            cout << "✓ Directed edge added: " << src_agb << " -> " << dest_agb << endl;
        } else {
            cout << "✗ Invalid vertices!" << endl;
        }
    }

    void BFS_agb(int startVertex_agb) {
        if (startVertex_agb < 0 || startVertex_agb >= vertices_agb) {
            cout << "Invalid start vertex!" << endl;
            return;
        }

        bool visited_agb[MAX_agb] = {false};
        queue<int> q_agb;

        cout << "\n--- BFS Traversal starting from vertex " << startVertex_agb << " ---" << endl;
        cout << "Order: ";

        visited_agb[startVertex_agb] = true;
        q_agb.push(startVertex_agb);

        while (!q_agb.empty()) {
            int vertex_agb = q_agb.front();
            q_agb.pop();
            cout << vertex_agb << " ";

            for (int i_agb = 0; i_agb < vertices_agb; i_agb++) {
                if (adjMatrix_agb[vertex_agb][i_agb] == 1 && !visited_agb[i_agb]) {
                    visited_agb[i_agb] = true;
                    q_agb.push(i_agb);
                }
            }
        }
        cout << endl;
    }

    void DFS_agb(int startVertex_agb) {
        if (startVertex_agb < 0 || startVertex_agb >= vertices_agb) {
            cout << "Invalid start vertex!" << endl;
            return;
        }

        bool visited_agb[MAX_agb] = {false};

        cout << "\n--- DFS Traversal starting from vertex " << startVertex_agb << " ---" << endl;
        cout << "Order: ";

        DFSRecursive_agb(startVertex_agb, visited_agb);
        cout << endl;
    }

    void displayMatrix_agb() {
        cout << "\n--- Adjacency Matrix ---" << endl;
        cout << "   ";
        for (int i_agb = 0; i_agb < vertices_agb; i_agb++) {
            cout << setw(3) << i_agb;
        }
        cout << endl;

        for (int i_agb = 0; i_agb < vertices_agb; i_agb++) {
            cout << setw(2) << i_agb << " ";
            for (int j_agb = 0; j_agb < vertices_agb; j_agb++) {
                cout << setw(3) << adjMatrix_agb[i_agb][j_agb];
            }
            cout << endl;
        }
    }

    void displayEdges_agb() {
        cout << "\n--- Graph Edges ---" << endl;
        bool hasEdges_agb = false;

        for (int i_agb = 0; i_agb < vertices_agb; i_agb++) {
            cout << "Vertex " << i_agb << " connected to: ";
            bool hasConnection_agb = false;

            for (int j_agb = 0; j_agb < vertices_agb; j_agb++) {
                if (adjMatrix_agb[i_agb][j_agb] == 1) {
                    cout << j_agb << " ";
                    hasConnection_agb = true;
                    hasEdges_agb = true;
                }
            }

            if (!hasConnection_agb) {
                cout << "None";
            }
            cout << endl;
        }

        if (!hasEdges_agb) {
            cout << "No edges in graph!" << endl;
        }
    }
};

int main_agb() {
    int vertices_agb, choice_agb;

    cout << "===== Graph Traversal (BFS & DFS) =====" << endl;
    cout << "Enter number of vertices: ";
    cin >> vertices_agb;

    Graph_agb graph_agb(vertices_agb);

    do {
        cout << "\n===== Menu =====" << endl;
        cout << "1. Add undirected edge" << endl;
        cout << "2. Add directed edge" << endl;
        cout << "3. Display adjacency matrix" << endl;
        cout << "4. Display edges" << endl;
        cout << "5. BFS Traversal" << endl;
        cout << "6. DFS Traversal" << endl;
        cout << "7. Create sample graph" << endl;
        cout << "8. Exit" << endl;
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
                graph_agb.displayMatrix_agb();
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

            case 7: {
                cout << "Creating sample graph..." << endl;
                cout << "Graph structure:" << endl;
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

            case 8:
                cout << "Exiting... Thank you!" << endl;
                break;

            default:
                cout << "Invalid choice!" << endl;
        }
    } while (choice_agb != 8);

    return 0;
}
```

---

## Input/Output

### Sample Run:

```
===== Graph Traversal (BFS & DFS) =====
Enter number of vertices: 6

===== Menu =====
Enter your choice: 7
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

--- Adjacency Matrix ---
     0  1  2  3  4  5
 0   0  1  0  1  0  0
 1   1  0  1  0  1  0
 2   0  1  0  0  0  1
 3   1  0  0  0  1  0
 4   0  1  0  1  0  1
 5   0  0  1  0  1  0

Enter your choice: 4

--- Graph Edges ---
Vertex 0 connected to: 1 3
Vertex 1 connected to: 0 2 4
Vertex 2 connected to: 1 5
Vertex 3 connected to: 0 4
Vertex 4 connected to: 1 3 5
Vertex 5 connected to: 2 4

Enter your choice: 5
Enter starting vertex for BFS: 0

--- BFS Traversal starting from vertex 0 ---
Order: 0 1 3 2 4 5

Enter your choice: 6
Enter starting vertex for DFS: 0

--- DFS Traversal starting from vertex 0 ---
Order: 0 1 2 5 4 3

Enter your choice: 8
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
│  │  └─ DFS(5): visited[5] = T, output: 5
│  │     └─ DFS(4): visited[4] = T, output: 4
│  │        └─ DFS(3): visited[3] = T, output: 3
│  │           └─ (all neighbors visited)
│  └─ (returns)
└─ (returns)
```

**Step-by-Step:**

1. Visit 0, mark visited → Output: 0
2. Go to first neighbor 1 → Output: 0 1
3. From 1, go to first unvisited neighbor 2 → Output: 0 1 2
4. From 2, go to first unvisited neighbor 5 → Output: 0 1 2 5
5. From 5, go to first unvisited neighbor 4 → Output: 0 1 2 5 4
6. From 4, go to first unvisited neighbor 3 → Output: 0 1 2 5 4 3
7. From 3, all neighbors visited, backtrack

**DFS Result:** 0 1 2 5 4 3

---

## Conclusion

This program successfully implements **Graph Traversal using Adjacency Matrix** with the following achievements:

1. **Adjacency Matrix Representation**:

   - 2D array of size V × V
   - adjMatrix[i][j] = 1 if edge exists
   - Space complexity: O(V²)
   - Efficient for dense graphs

2. **BFS (Breadth-First Search)**:

   - Uses queue data structure
   - Explores level by level
   - Shortest path in unweighted graphs
   - Time complexity: O(V²) with adjacency matrix

3. **DFS (Depth-First Search)**:

   - Uses recursion (implicit stack)
   - Explores as far as possible before backtracking
   - Used for cycle detection, topological sort
   - Time complexity: O(V²) with adjacency matrix

4. **Key Differences**:
   | Feature | BFS | DFS |
   |---------|-----|-----|
   | Data Structure | Queue | Stack (Recursion) |
   | Order | Level-wise | Depth-wise |
   | Memory | O(V) | O(h) where h is height |
   | Path | Shortest | Any path |

5. **Applications**:

   - **BFS**: Shortest path, web crawling, social networks
   - **DFS**: Cycle detection, maze solving, topological sort

6. **Adjacency Matrix Advantages**:

   - O(1) edge lookup
   - Simple implementation
   - Good for dense graphs

7. **Adjacency Matrix Disadvantages**:
   - O(V²) space even for sparse graphs
   - O(V) time to find all neighbors

**Time Complexity**:

- BFS: O(V²) using adjacency matrix
- DFS: O(V²) using adjacency matrix
- Add edge: O(1)

**Space Complexity**:

- Matrix: O(V²)
- Visited array: O(V)
- BFS queue: O(V)
- DFS stack: O(V)
