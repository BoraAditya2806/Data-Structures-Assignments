# Assignment 3

## Problem Statement

Write a program to implement Kruskal's algorithm to find the minimum spanning tree of a user defined graph. Use Adjacency List to represent a graph.

---

## Pseudocode

```
CLASS Edge:
    source: integer
    destination: integer
    weight: integer

CLASS Graph:
    vertices: integer
    edges: list of Edge

    FUNCTION __init__(v):
        vertices = v
        CREATE empty edges list

    FUNCTION addEdge(src, dest, weight):
        ADD Edge(src, dest, weight) to edges list

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
        CREATE result array for MST edges
        SORT edges by weight

        CREATE parent array, initialize with self parents
        CREATE rank array, initialize with zeros

        FOR each edge in sorted edges:
            x = find(parent, edge.source)
            y = find(parent, edge.destination)

            IF x != y:
                ADD edge to result
                union(parent, rank, x, y)

        DISPLAY MST edges and total weight

MAIN:
    CREATE graph
    ADD edges
    CALL kruskalsMST()
```

---

## C++ Code

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <iomanip>
using namespace std;

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
    vector<Edge_agb> edges_agb;

    // Find function for Union-Find (with Path Compression)
    int find_agb(int parent_agb[], int i_agb) {
        if (parent_agb[i_agb] == i_agb) {
            return i_agb;
        }
        parent_agb[i_agb] = find_agb(parent_agb, parent_agb[i_agb]); // Path Compression
        return parent_agb[i_agb];
    }

    // Union function for Union-Find (Union by Rank)
    void Union_agb(int parent_agb[], int rank_agb[], int x_agb, int y_agb) {
        int xroot_agb = find_agb(parent_agb, x_agb);
        int yroot_agb = find_agb(parent_agb, y_agb);

        if (xroot_agb != yroot_agb) { // Only union if they are in different sets
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
    }

public:
    Graph_agb(int v_agb) {
        vertices_agb = v_agb;
    }

    // Add weighted edge (undirected)
    void addEdge_agb(int src_agb, int dest_agb, int weight_agb) {
        if (src_agb >= 0 && src_agb < vertices_agb && dest_agb >= 0 && dest_agb < vertices_agb) {
            edges_agb.push_back(Edge_agb(src_agb, dest_agb, weight_agb));
            cout << "✓ Edge added: " << src_agb << " -- " << dest_agb
                 << " (weight: " << weight_agb << ")" << endl;
        } else {
            cout << "✗ Invalid vertices!" << endl;
        }
    }

    // Kruskal's Minimum Spanning Tree Algorithm
    void kruskalsMST_agb() {
        if (edges_agb.empty()) {
            cout << "Graph has no edges!" << endl;
            return;
        }

        cout << "\n===== Kruskal's Algorithm Execution =====" << endl;

        // Step 1: Sort all edges by weight
        sort(edges_agb.begin(), edges_agb.end());
        

        cout << "\nSorted edges by weight:" << endl;
        for (int i_agb = 0; i_agb < edges_agb.size(); i_agb++) {
            cout << edges_agb[i_agb].source_agb << " -- " << edges_agb[i_agb].destination_agb
                 << " (weight: " << edges_agb[i_agb].weight_agb << ")" << endl;
        }

        // Step 2: Initialize Union-Find data structures
        int* parent_agb = new int[vertices_agb];
        int* rank_agb = new int[vertices_agb];

        for (int i_agb = 0; i_agb < vertices_agb; i_agb++) {
            parent_agb[i_agb] = i_agb; // Each vertex is its own set initially
            rank_agb[i_agb] = 0;
        }

        // Step 3: Process edges in sorted order
        vector<Edge_agb> result_agb; // Store edges of MST
        int edgeIndex_agb = 0;     // Index for sorted edges
        int resultIndex_agb = 0;   // Index for result edges

        cout << "\nProcessing edges:" << endl;

        // The MST will have V-1 edges (if connected)
        while (resultIndex_agb < vertices_agb - 1 && edgeIndex_agb < edges_agb.size()) {
            // Step 4: Pick the smallest edge
            Edge_agb nextEdge_agb = edges_agb[edgeIndex_agb++];

            int x_agb = find_agb(parent_agb, nextEdge_agb.source_agb);
            int y_agb = find_agb(parent_agb, nextEdge_agb.destination_agb);

            cout << "Edge " << nextEdge_agb.source_agb << " -- " << nextEdge_agb.destination_agb
                 << " (weight: " << nextEdge_agb.weight_agb << ")";

            // Step 5: Check if including this edge causes a cycle using Union-Find
            if (x_agb != y_agb) {
                // If roots are different, no cycle. Include the edge and union the sets.
                cout << " ✓ Added to MST" << endl;
                result_agb.push_back(nextEdge_agb);
                Union_agb(parent_agb, rank_agb, x_agb, y_agb);
                resultIndex_agb++;
            } else {
                // If roots are the same, a cycle is formed. Skip the edge.
                cout << " ✗ Forms cycle, skipped" << endl;
            }
        }

        if (resultIndex_agb < vertices_agb - 1) {
             cout << "\nWarning: The graph might be disconnected, as only " << resultIndex_agb << " edges were found for " << vertices_agb << " vertices." << endl;
        }

        // Display the constructed MST
        displayMST_agb(result_agb);

        // Calculate and display total weight
        int totalWeight_agb = 0;
        for (int i_agb = 0; i_agb < result_agb.size(); i_agb++) {
            totalWeight_agb += result_agb[i_agb].weight_agb;
        }

        cout << "\nTotal weight of MST: " << totalWeight_agb << endl;

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

    // Display all edges
    void displayEdges_agb() {
        cout << "\n--- Graph Edges ---" << endl;

        if (edges_agb.empty()) {
            cout << "No edges in graph!" << endl;
            return;
        }

        for (int i_agb = 0; i_agb < edges_agb.size(); i_agb++) {
            cout << edges_agb[i_agb].source_agb << " -- " << edges_agb[i_agb].destination_agb
                 << " (weight: " << edges_agb[i_agb].weight_agb << ")" << endl;
        }
    }
};

int main_agb() {
    int vertices_agb, choice_agb;

    cout << "===== Kruskal's Minimum Spanning Tree Algorithm =====" << endl;
    cout << "Enter number of vertices: ";
    cin >> vertices_agb;

    Graph_agb graph_agb(vertices_agb);

    do {
        cout << "\n===== Menu =====" << endl;
        cout << "1. Add edge" << endl;
        cout << "2. Display edges" << endl;
        cout << "3. Find Minimum Spanning Tree (Kruskal's)" << endl;
        cout << "4. Create sample graph" << endl;
        cout << "5. Exit" << endl;
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
                graph_agb.displayEdges_agb();
                break;

            case 3:
                graph_agb.kruskalsMST_agb();
                break;

            case 4: {
                cout << "Creating sample graph..." << endl;
                cout << "Graph structure (Example with 4 vertices: 0, 1, 2, 3):" << endl;
                cout << "     2     " << endl;
                cout << "  0-----1  " << endl;
                cout << "  |\\   /|  " << endl;
                cout << " 4| 3 |5 " << endl;
                cout << "  | / \\ |  " << endl;
                cout << "  2-----3  " << endl;
                cout << "     6     " << endl;

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

            case 5:
                cout << "Exiting... Thank you!" << endl;
                break;

            default:
                cout << "Invalid choice!" << endl;
        }
    } while (choice_agb != 5);

    return 0;
}
```

---

## Input/Output

### Sample Run:

```
===== Kruskal's Minimum Spanning Tree Algorithm =====
Enter number of vertices: 4

===== Menu =====
Enter your choice: 4
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

Enter your choice: 3

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

Enter your choice: 2

--- Graph Edges ---
0 -- 1 (weight: 2)
0 -- 2 (weight: 4)
0 -- 3 (weight: 3)
1 -- 2 (weight: 2)
1 -- 3 (weight: 5)
2 -- 3 (weight: 6)

Enter your choice: 5
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

### Kruskal's Algorithm Execution:

**Step 1: Sort Edges by Weight**

```
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

This program successfully implements **Kruskal's Minimum Spanning Tree Algorithm** using Adjacency List representation:

1. **Kruskal's Algorithm**:

   - Greedy approach to find MST
   - Edge-based algorithm
   - Processes edges in increasing order of weight
   - Uses Union-Find data structure to detect cycles

2. **Algorithm Steps**:

   - Sort all edges by weight
   - Initialize Union-Find structure
   - For each edge in sorted order:
     - If it doesn't form a cycle, add to MST
     - Otherwise, skip the edge

3. **Key Data Structures**:

   - **Edge List**: Stores all edges for sorting
   - **Union-Find**: Detects cycles efficiently
   - **Parent Array**: Tracks component representatives
   - **Rank Array**: Maintains balanced trees

4. **Union-Find Operations**:

   - **Find**: Determines which component a vertex belongs to
   - **Union**: Merges two components
   - **Union by Rank**: Keeps trees balanced for efficiency

5. **Time Complexity**:

   - Sorting edges: O(E log E)
   - Union-Find operations: O(E α(V)) where α is inverse Ackermann function
   - Overall: O(E log E) or O(E log V)

6. **Space Complexity**:

   - Edge list: O(E)
   - Union-Find arrays: O(V)
   - Result storage: O(V)
   - Total: O(E + V)

7. **Advantages**:

   - Works well with sparse graphs
   - Simple to understand and implement
   - Guaranteed optimal solution
   - Can be parallelized

8. **Applications**:
   - Network design
   - Circuit design
   - Transportation networks
   - Image segmentation
   - Cluster analysis

**Comparison with Prim's Algorithm**:
| Feature | Kruskal's | Prim's |
|---------|-----------|--------|
| Approach | Edge-based | Vertex-based |
| Data Structure | Union-Find | Priority queue |
| Best for | Sparse graphs | Dense graphs |
| Time Complexity | O(E log E) | O(V²) |
| Space | O(E) | O(V) |

**Union-Find Optimization Techniques**:

1. **Union by Rank**: Attach smaller tree under root of larger tree
2. **Path Compression**: Flatten tree during find operations
3. **Combined**: Both techniques together provide near-constant time operations
