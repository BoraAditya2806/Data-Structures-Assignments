# Assignment 6

## Problem Statement

Write a program to implement Kruskal's algorithm to find the minimum spanning tree of a user defined graph. Use Adjacency Matrix to represent a graph.

---

## Pseudocode

```
CLASS Edge:
    source: integer
    destination: integer
    weight: integer

CLASS Graph:
    vertices: integer
    adjMatrix: 2D array

    FUNCTION __init__(v):
        vertices = v
        CREATE adjMatrix of size v×v, initialize with 0

    FUNCTION addEdge(src, dest, weight):
        adjMatrix[src][dest] = weight
        adjMatrix[dest][src] = weight  // For undirected

    FUNCTION getAllEdges():
        CREATE edges list
        FOR i = 0 to vertices-1:
            FOR j = i+1 to vertices-1:
                IF adjMatrix[i][j] > 0:
                    ADD Edge(i, j, adjMatrix[i][j]) to edges
        RETURN edges

    FUNCTION find(parent, i):
        IF parent[i] == i:
            RETURN i
        RETURN find(parent, parent[i])

    FUNCTION union(parent, rank, x, y):
        xroot = find(parent, x)
        yroot = find(parent, y)

        IF rank[xroot] < rank[yroot]:
            parent[xroot] = yroot
        ELSE IF rank[xroot] > rank[yroot]:
            parent[yroot] = xroot
        ELSE:
            parent[yroot] = xroot
            rank[xroot]++

    FUNCTION kruskalsMST():
        edges = getAllEdges()
        SORT edges by weight

        CREATE parent array, initialize with self parents
        CREATE rank array, initialize with zeros

        CREATE result array for MST edges

        FOR each edge in sorted edges:
            x = find(parent, edge.source)
            y = find(parent, edge.destination)

            IF x != y:
                ADD edge to result
                union(parent, rank, x, y)

        DISPLAY MST edges and total weight

MAIN:
    CREATE graph
    ADD edges with weights
    CALL kruskalsMST()
```

---

## C++ Code

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <climits>
#include <iomanip>
using namespace std;

#define MAX_agb 20

class Edge_agb {
public:
    int source_agb;
    int destination_agb;
    int weight_agb;

    Edge_agb(int src_agb, int dest_agb, int w_agb) {
        source_agb = src_agb;
        destination_agb = dest_agb;
        weight_agb = w_agb;
    }

    // Operator for sorting edges by weight
    bool operator<(const Edge_agb& other_agb) const {
        return weight_agb < other_agb.weight_agb;
    }
};

class Graph_agb {
private:
    int vertices_agb;
    int adjMatrix_agb[MAX_agb][MAX_agb];

    // Find function for Union-Find with Path Compression
    int find_agb(int parent_agb[], int i_agb) {
        if (parent_agb[i_agb] == i_agb) {
            return i_agb;
        }
        // Path Compression
        return parent_agb[i_agb] = find_agb(parent_agb, parent_agb[i_agb]);
    }

    // Union function for Union-Find (Union by Rank)
    void Union_agb(int parent_agb[], int rank_agb[], int x_agb, int y_agb) {
        int xroot_agb = find_agb(parent_agb, x_agb);
        int yroot_agb = find_agb(parent_agb, y_agb);

        // Attach smaller rank tree under root of higher rank tree
        if (rank_agb[xroot_agb] < rank_agb[yroot_agb]) {
            parent_agb[xroot_agb] = yroot_agb;
        } else if (rank_agb[xroot_agb] > rank_agb[yroot_agb]) {
            parent_agb[yroot_agb] = xroot_agb;
        } else {
            // If ranks are same, make one as root and increment its rank
            parent_agb[yroot_agb] = xroot_agb;
            rank_agb[xroot_agb]++;
        }
    }

    // Get all edges from adjacency matrix
    vector<Edge_agb> getAllEdges_agb() {
        vector<Edge_agb> edges_agb;

        for (int i_agb = 0; i_agb < vertices_agb; i_agb++) {
            for (int j_agb = i_agb + 1; j_agb < vertices_agb; j_agb++) {
                if (adjMatrix_agb[i_agb][j_agb] > 0) {
                    edges_agb.push_back(Edge_agb(i_agb, j_agb, adjMatrix_agb[i_agb][j_agb]));
                }
            }
        }

        return edges_agb;
    }

public:
    Graph_agb(int v_agb) {
        vertices_agb = v_agb;

        // Initialize adjacency matrix with 0s
        for (int i_agb = 0; i_agb < vertices_agb; i_agb++) {
            for (int j_agb = 0; j_agb < vertices_agb; j_agb++) {
                adjMatrix_agb[i_agb][j_agb] = 0;
            }
        }
    }

    // Add weighted edge (undirected)
    void addEdge_agb(int src_agb, int dest_agb, int weight_agb) {
        if (src_agb >= 0 && src_agb < vertices_agb && dest_agb >= 0 && dest_agb < vertices_agb) {
            adjMatrix_agb[src_agb][dest_agb] = weight_agb;
            adjMatrix_agb[dest_agb][src_agb] = weight_agb;
            cout << "✓ Edge added: " << src_agb << " -- " << dest_agb
                 << " (weight: " << weight_agb << ")" << endl;
        } else {
            cout << "✗ Invalid vertices!" << endl;
        }
    }

    // Kruskal's Minimum Spanning Tree Algorithm
    void kruskalsMST_agb() {
        // Check if graph has edges
        bool hasEdges_agb = false;
        for (int i_agb = 0; i_agb < vertices_agb; i_agb++) {
            for (int j_agb = 0; j_agb < vertices_agb; j_agb++) {
                if (adjMatrix_agb[i_agb][j_agb] > 0) {
                    hasEdges_agb = true;
                    break;
                }
            }
            if (hasEdges_agb) break;
        }

        if (!hasEdges_agb) {
            cout << "Graph has no edges!" << endl;
            return;
        }

        cout << "\n===== Kruskal's Algorithm Execution =====" << endl;
        


        // Step 1: Get all edges and sort them
        vector<Edge_agb> edges_agb = getAllEdges_agb();
        sort(edges_agb.begin(), edges_agb.end());

        cout << "\nSorted edges by weight:" << endl;
        for (int i_agb = 0; i_agb < edges_agb.size(); i_agb++) {
            cout << edges_agb[i_agb].source_agb << " -- " << edges_agb[i_agb].destination_agb
                 << " (weight: " << edges_agb[i_agb].weight_agb << ")" << endl;
        }

        // Step 2: Initialize Union-Find data structures
        // parent_agb[i] stores the parent of vertex i
        int* parent_agb = new int[vertices_agb];
        // rank_agb[i] stores the rank (or approximate height) of the tree rooted at i
        int* rank_agb = new int[vertices_agb];

        for (int i_agb = 0; i_agb < vertices_agb; i_agb++) {
            parent_agb[i_agb] = i_agb; // Each vertex is its own parent initially (a separate set)
            rank_agb[i_agb] = 0;
        }

        // Step 3: Process edges in sorted order
        vector<Edge_agb> result_agb;  // Store edges of MST
        int edgeIndex_agb = 0;    // Index for sorted edges
        int resultIndex_agb = 0;  // Count of edges added to MST (should be V-1)

        cout << "\nProcessing edges:" << endl;

        // Number of edges in MST is (V-1)
        while (resultIndex_agb < vertices_agb - 1 && edgeIndex_agb < edges_agb.size()) {
            // Step 4: Pick the smallest edge
            Edge_agb nextEdge_agb = edges_agb[edgeIndex_agb++];

            // Find the sets (roots) of the two vertices
            int x_agb = find_agb(parent_agb, nextEdge_agb.source_agb);
            int y_agb = find_agb(parent_agb, nextEdge_agb.destination_agb);

            cout << "Edge " << nextEdge_agb.source_agb << " -- " << nextEdge_agb.destination_agb
                 << " (weight: " << nextEdge_agb.weight_agb << ")";

            // Step 5: If including this edge doesn't cause cycle (i.e., they belong to different sets), include it in MST
            if (x_agb != y_agb) {
                cout << " ✓ **Added to MST**" << endl;
                result_agb.push_back(nextEdge_agb);
                Union_agb(parent_agb, rank_agb, x_agb, y_agb); // Merge the two sets
                resultIndex_agb++;
            } else {
                cout << " ✗ **Forms cycle**, skipped" << endl;
            }
        }
        
        // Final check for connectivity (if the graph is not connected, the MST won't have V-1 edges)
        if (resultIndex_agb != vertices_agb - 1 && vertices_agb > 1) {
            cout << "\n**Warning:** The graph is disconnected. A spanning forest (not a single MST) was found." << endl;
        }

        // Display the constructed MST
        displayMST_agb(result_agb);

        // Calculate and display total weight
        int totalWeight_agb = 0;
        for (int i_agb = 0; i_agb < result_agb.size(); i_agb++) {
            totalWeight_agb += result_agb[i_agb].weight_agb;
        }

        cout << "\n**Total weight of MST:** " << totalWeight_agb << endl;

        // Cleanup
        delete[] parent_agb;
        delete[] rank_agb;
    }

    // Display the MST
    void displayMST_agb(vector<Edge_agb>& result_agb) {
        cout << "\n========== Minimum Spanning Tree Edges ==========" << endl;
        cout << setw(10) << "Edge" << setw(15) << "Weight" << endl;
        cout << string(25, '-') << endl;

        for (int i_agb = 0; i_agb < result_agb.size(); i_agb++) {
            cout << setw(5) << result_agb[i_agb].source_agb << " - " << result_agb[i_agb].destination_agb
                 << setw(15) << result_agb[i_agb].weight_agb << endl;
        }

        cout << string(25, '=') << endl;
    }

    // Display adjacency matrix
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

    // Display graph edges
    void displayEdges_agb() {
        cout << "\n--- Graph Edges ---" << endl;
        bool hasEdges_agb = false;

        for (int i_agb = 0; i_agb < vertices_agb; i_agb++) {
            for (int j_agb = i_agb + 1; j_agb < vertices_agb; j_agb++) {
                if (adjMatrix_agb[i_agb][j_agb] > 0) {
                    cout << i_agb << " -- " << j_agb
                         << " (weight: " << adjMatrix_agb[i_agb][j_agb] << ")" << endl;
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

    cout << "===== Kruskal's MST with Adjacency Matrix =====" << endl;
    cout << "Enter number of vertices: ";
    cin >> vertices_agb;

    if (vertices_agb > MAX_agb) {
        cout << "Maximum vertices allowed: " << MAX_agb << endl;
        return 1;
    }

    Graph_agb graph_agb(vertices_agb);

    do {
        cout << "\n===== Menu =====" << endl;
        cout << "1. Add edge" << endl;
        cout << "2. Display adjacency matrix" << endl;
        cout << "3. Display edges" << endl;
        cout << "4. Find Minimum Spanning Tree (Kruskal's)" << endl;
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
                graph_agb.displayMatrix_agb();
                break;

            case 3:
                graph_agb.displayEdges_agb();
                break;

            case 4:
                graph_agb.kruskalsMST_agb();
                break;

            case 5: {
                cout << "Creating sample graph..." << endl;
                cout << "Graph structure (4 vertices: 0, 1, 2, 3):" << endl;
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
                    cout << "Need at least 4 vertices to create the sample graph." << endl;
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
===== Kruskal's MST with Adjacency Matrix =====
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

Enter your choice: 2

--- Adjacency Matrix ---
     0  1  2  3
 0   0  2  4  3
 1   2  0  2  5
 2   4  2  0  6
 3   3  5  6  0

Enter your choice: 4

===== Kruskal's Algorithm Execution =====

Sorted edges by weight:
0 -- 1 (weight: 2)
1 -- 2 (weight: 2)
0 -- 3 (weight: 3)
0 -- 2 (weight: 4)
1 -- 3 (weight: 5)
2 -- 3 (weight: 6)

Processing edges:
Edge 0 -- 1 (weight: 2) ✓ Added to MST
Edge 1 -- 2 (weight: 2) ✓ Added to MST
Edge 0 -- 3 (weight: 3) ✓ Added to MST
Edge 0 -- 2 (weight: 4) ✗ Forms cycle, skipped
Edge 1 -- 3 (weight: 5) ✗ Forms cycle, skipped
Edge 2 -- 3 (weight: 6) ✗ Forms cycle, skipped

========== Minimum Spanning Tree Edges ==========
      Edge         Weight
-------------------------
    0 - 1              2
    1 - 2              2
    0 - 3              3
=========================

Total weight of MST: 7

Enter your choice: 3

--- Graph Edges ---
0 -- 1 (weight: 2)
0 -- 2 (weight: 4)
0 -- 3 (weight: 3)
1 -- 2 (weight: 2)
1 -- 3 (weight: 5)
2 -- 3 (weight: 6)

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
Adjacency Matrix:
     0  1  2  3
 0   0  2  4  3
 1   2  0  2  5
 2   4  2  0  6
 3   3  5  6  0
```

### Kruskal's Algorithm Execution:

**Step 1: Extract and Sort Edges**

```
Edges from matrix: [0-1(2), 0-2(4), 0-3(3), 1-2(2), 1-3(5), 2-3(6)]
Sorted edges: [0-1(2), 1-2(2), 0-3(3), 0-2(4), 1-3(5), 2-3(6)]
```

**Step 2: Initialize Union-Find**

```
parent[] = [0, 1, 2, 3]  // Each vertex is its own parent initially
rank[] = [0, 0, 0, 0]    // All ranks are 0
```

**Step 3: Process Edges**

**Edge 0-1 (weight 2):**

```
find(0) = 0, find(1) = 1
0 ≠ 1 → No cycle, add to MST
Union(0, 1):
  rank[0] = rank[1] → parent[1] = 0, rank[0] = 1

MST: [0-1(2)]
parent[] = [0, 0, 2, 3]
rank[] = [1, 0, 0, 0]
```

**Edge 1-2 (weight 2):**

```
find(1) = find(parent[1]) = find(0) = 0
find(2) = 2
0 ≠ 2 → No cycle, add to MST
Union(0, 2):
  rank[0] = 1 > rank[2] = 0 → parent[2] = 0

MST: [0-1(2), 1-2(2)]
parent[] = [0, 0, 0, 3]
rank[] = [1, 0, 0, 0]
```

**Edge 0-3 (weight 3):**

```
find(0) = 0
find(3) = 3
0 ≠ 3 → No cycle, add to MST
Union(0, 3):
  rank[0] = 1 > rank[3] = 0 → parent[3] = 0

MST: [0-1(2), 1-2(2), 0-3(3)]
parent[] = [0, 0, 0, 0]
rank[] = [1, 0, 0, 0]
```

**Edge 0-2 (weight 4):**

```
find(0) = 0
find(2) = find(parent[2]) = find(0) = 0
0 = 0 → Forms cycle, skip
```

**Edge 1-3 (weight 5):**

```
find(1) = find(parent[1]) = find(0) = 0
find(3) = find(parent[3]) = find(0) = 0
0 = 0 → Forms cycle, skip
```

**Edge 2-3 (weight 6):**

```
find(2) = find(parent[2]) = find(0) = 0
find(3) = find(parent[3]) = find(0) = 0
0 = 0 → Forms cycle, skip
```

**Final MST:**

```
Edges: 0-1(2), 1-2(2), 0-3(3)
Total weight: 2 + 2 + 3 = 7
```

---

## Conclusion

This program successfully implements **Kruskal's Minimum Spanning Tree Algorithm** using Adjacency Matrix representation:

1. **Kruskal's Algorithm with Adjacency Matrix**:

   - Edge-based greedy algorithm
   - Processes edges in increasing order of weight
   - Uses Union-Find data structure to detect cycles
   - Works with adjacency matrix representation

2. **Algorithm Steps**:

   - Extract all edges from adjacency matrix
   - Sort edges by weight
   - Initialize Union-Find structure
   - Process edges in sorted order:
     - If edge doesn't form cycle, add to MST
     - Otherwise, skip the edge

3. **Key Data Structures**:

   - **Adjacency Matrix**: O(V²) space for graph representation
   - **Edge List**: Extracted edges for sorting
   - **Union-Find**: Cycle detection with path compression
   - **Parent Array**: Component representatives
   - **Rank Array**: Balanced tree maintenance

4. **Union-Find Operations**:

   - **Find**: O(α(V)) with path compression
   - **Union**: O(α(V)) with union by rank
   - **α(V)**: Inverse Ackermann function (practically constant)

5. **Time Complexity**:

   - Extracting edges: O(V²)
   - Sorting edges: O(E log E) = O(V² log V)
   - Union-Find operations: O(E α(V)) = O(V²)
   - Overall: O(V² log V)

6. **Space Complexity**:

   - Adjacency matrix: O(V²)
   - Edge list: O(V²)
   - Union-Find arrays: O(V)
   - Result storage: O(V)
   - Total: O(V²)

7. **Advantages**:

   - Simple to understand and implement
   - Guaranteed optimal solution
   - Works with disconnected graphs
   - Easy to modify for directed graphs

8. **Disadvantages**:
   - Space inefficient for sparse graphs
   - Slower than adjacency list for sparse graphs
   - Extracting edges takes O(V²) time

**Comparison with Adjacency List Implementation**:
| Feature | Adjacency Matrix | Adjacency List |
|---------|------------------|----------------|
| Space | O(V²) | O(V + E) |
| Edge Extraction | O(V²) | O(V + E) |
| Best for | Dense graphs | Sparse graphs |
| Implementation | Simpler | More complex |

**Applications**:

- Network design
- Circuit design
- Transportation networks
- Image segmentation
- Cluster analysis

**Optimization Techniques**:

1. **Path Compression**: Flatten tree during find operations
2. **Union by Rank**: Keep trees balanced
3. **Early Termination**: Stop after V-1 edges
4. **Edge Filtering**: Skip zero-weight edges
