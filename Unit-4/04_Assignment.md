# Assignment 4

## Problem Statement

Write a Program to create a Binary Tree and perform following Non-recursive operations on it:

- a. Inorder Traversal
- b. Preorder Traversal
- c. Display Number of Leaf Nodes
- d. Mirror Image

---

## Pseudocode

```
CLASS Node:
    data: integer
    left: pointer to Node
    right: pointer to Node

CLASS BST:
    root: pointer to Node

    FUNCTION insert(value):
        root = insertRecursive(root, value)

    FUNCTION inorderIterative():
        CREATE stack
        current = root

        WHILE current != NULL OR stack is not empty:
            WHILE current != NULL:
                stack.push(current)
                current = current.left

            current = stack.pop()
            PRINT current.data
            current = current.right

    FUNCTION preorderIterative():
        IF root == NULL:
            RETURN

        CREATE stack
        stack.push(root)

        WHILE stack is not empty:
            current = stack.pop()
            PRINT current.data

            IF current.right != NULL:
                stack.push(current.right)
            IF current.left != NULL:
                stack.push(current.left)

    FUNCTION countLeafNodesIterative():
        IF root == NULL:
            RETURN 0

        CREATE queue
        queue.enqueue(root)
        count = 0

        WHILE queue is not empty:
            current = queue.dequeue()

            IF current.left == NULL AND current.right == NULL:
                count++

            IF current.left != NULL:
                queue.enqueue(current.left)
            IF current.right != NULL:
                queue.enqueue(current.right)

        RETURN count

    FUNCTION mirrorIterative():
        IF root == NULL:
            RETURN

        CREATE queue
        queue.enqueue(root)

        WHILE queue is not empty:
            current = queue.dequeue()

            // Swap left and right children
            temp = current.left
            current.left = current.right
            current.right = temp

            IF current.left != NULL:
                queue.enqueue(current.left)
            IF current.right != NULL:
                queue.enqueue(current.right)

MAIN:
    CREATE BST
    INSERT values
    PERFORM all non-recursive operations
```

---

## C++ Code

```cpp
#include <iostream>
#include <stack>
#include <queue>
using namespace std;

class Node {
public:
    int data;
    Node* left;
    Node* right;

    Node_agb(int value) {
        data = value;
        left = NULL;
        right = NULL;
    }
};

class BST {
private:
    Node* root;

    Node* insertRecursive_agb(Node* node, int value) {
        if (node == NULL) {
            return new Node_agb(value);
        }

        if (value < node->data) {
            node->left = insertRecursive_agb(node->left, value);
        } else if (value > node->data) {
            node->right = insertRecursive_agb(node->right, value);
        }

        return node;
    }

public:
    BST_agb() {
        root = NULL;
    }

    void insert_agb(int value) {
        root = insertRecursive_agb(root, value);
        cout << "✓ Inserted " << value << endl;
    }

    void inorderIterative_agb() {
        if (root == NULL) {
            cout << "Tree is empty!" << endl;
            return;
        }

        stack<Node*> st_agb;
        Node* current_agb = root;

        cout << "Inorder (Non-recursive): ";

        while (current_agb != NULL || !st_agb.empty()) {
            while (current_agb != NULL) {
                st_agb.push(current_agb);
                current_agb = current_agb->left;
            }

            current_agb = st_agb.top();
            st_agb.pop();
            cout << current_agb->data << " ";

            current_agb = current_agb->right;
        }
        cout << endl;
    }

    void preorderIterative_agb() {
        if (root == NULL) {
            cout << "Tree is empty!" << endl;
            return;
        }

        stack<Node*> st_agb;
        st_agb.push(root);

        cout << "Preorder (Non-recursive): ";

        while (!st_agb.empty()) {
            Node* current_agb = st_agb.top();
            st_agb.pop();

            cout << current_agb->data << " ";

            if (current_agb->right != NULL) {
                st_agb.push(current_agb->right);
            }
            if (current_agb->left != NULL) {
                st_agb.push(current_agb->left);
            }
        }
        cout << endl;
    }

    int countLeafNodesIterative_agb() {
        if (root == NULL) {
            return 0;
        }

        queue<Node*> q_agb;
        q_agb.push(root);
        int leafCount_agb = 0;

        while (!q_agb.empty()) {
            Node* current_agb = q_agb.front();
            q_agb.pop();

            if (current_agb->left == NULL && current_agb->right == NULL) {
                leafCount_agb++;
            }

            if (current_agb->left != NULL) {
                q_agb.push(current_agb->left);
            }
            if (current_agb->right != NULL) {
                q_agb.push(current_agb->right);
            }
        }

        return leafCount_agb;
    }

    void mirrorIterative_agb() {
        if (root == NULL) {
            cout << "Tree is empty!" << endl;
            return;
        }

        queue<Node*> q_agb;
        q_agb.push(root);

        cout << "✓ Mirror image created (Non-recursive)!" << endl;

        while (!q_agb.empty()) {
            Node* current_agb = q_agb.front();
            q_agb.pop();

            Node* temp_agb = current_agb->left;
            current_agb->left = current_agb->right;
            current_agb->right = temp_agb;

            if (current_agb->left != NULL) {
                q_agb.push(current_agb->left);
            }
            if (current_agb->right != NULL) {
                q_agb.push(current_agb->right);
            }
        }
    }

    void levelOrderIterative_agb() {
        if (root == NULL) {
            cout << "Tree is empty!" << endl;
            return;
        }

        queue<Node*> q_agb;
        q_agb.push(root);

        cout << "Level Order (Non-recursive): ";

        while (!q_agb.empty()) {
            Node* current_agb = q_agb.front();
            q_agb.pop();

            cout << current_agb->data << " ";

            if (current_agb->left != NULL) {
                q_agb.push(current_agb->left);
            }
            if (current_agb->right != NULL) {
                q_agb.push(current_agb->right);
            }
        }
        cout << endl;
    }

    int countNodesIterative_agb() {
        if (root == NULL) {
            return 0;
        }

        queue<Node*> q_agb;
        q_agb.push(root);
        int count_agb = 0;

        while (!q_agb.empty()) {
            Node* current_agb = q_agb.front();
            q_agb.pop();
            count_agb++;

            if (current_agb->left != NULL) {
                q_agb.push(current_agb->left);
            }
            if (current_agb->right != NULL) {
                q_agb.push(current_agb->right);
            }
        }

        return count_agb;
    }

    void displayTree_agb() {
        if (root == NULL) {
            cout << "Tree is empty!" << endl;
            return;
        }

        cout << "\nBST Structure:" << endl;
        displayTreeHelper_agb(root, "", false);
    }

    void displayTreeHelper_agb(Node* node, string prefix, bool isLeft) {
        if (node == NULL) return;

        cout << prefix;
        cout << (isLeft ? "├──" : "└──");
        cout << node->data << endl;

        if (node->left != NULL || node->right != NULL) {
            if (node->left != NULL) {
                displayTreeHelper_agb(node->left, prefix + (isLeft ? "│   " : "    "), true);
            } else if (node->right != NULL) {
                cout << prefix << (isLeft ? "│   " : "    ") << "├──NULL" << endl;
            }

            if (node->right != NULL) {
                displayTreeHelper_agb(node->right, prefix + (isLeft ? "│   " : "    "), false);
            } else if (node->left != NULL) {
                cout << prefix << (isLeft ? "│   " : "    ") << "└──NULL" << endl;
            }
        }
    }
};

int main_agb() {
    BST_agb tree_agb;
    int choice, value;

    do {
        cout << "\n===== BST Non-Recursive Operations =====" << endl;
        cout << "1. Insert node" << endl;
        cout << "2. Inorder traversal (Non-recursive)" << endl;
        cout << "3. Preorder traversal (Non-recursive)" << endl;
        cout << "4. Level order traversal (Non-recursive)" << endl;
        cout << "5. Count leaf nodes (Non-recursive)" << endl;
        cout << "6. Count total nodes (Non-recursive)" << endl;
        cout << "7. Create mirror image (Non-recursive)" << endl;
        cout << "8. Display tree structure" << endl;
        cout << "9. Create sample tree" << endl;
        cout << "10. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter value to insert: ";
                cin >> value;
                tree_agb.insert_agb(value);
                break;

            case 2:
                tree_agb.inorderIterative_agb();
                break;

            case 3:
                tree_agb.preorderIterative_agb();
                break;

            case 4:
                tree_agb.levelOrderIterative_agb();
                break;

            case 5:
                cout << "\nNumber of leaf nodes: " << tree_agb.countLeafNodesIterative_agb() << endl;
                break;

            case 6:
                cout << "\nTotal number of nodes: " << tree_agb.countNodesIterative_agb() << endl;
                break;

            case 7:
                tree_agb.mirrorIterative_agb();
                break;

            case 8:
                tree_agb.displayTree_agb();
                break;

            case 9: {
                int values_agb[] = {50, 30, 70, 20, 40, 60, 80, 10, 25, 35, 45};
                cout << "Creating sample tree with values: ";
                for (int i = 0; i < 11; i++) {
                    cout << values_agb[i] << " ";
                }
                cout << endl;

                for (int i = 0; i < 11; i++) {
                    tree_agb.insert_agb(values_agb[i]);
                }
                break;
            }

            case 10:
                cout << "Exiting... Thank you!" << endl;
                break;

            default:
                cout << "Invalid choice!" << endl;
        }
    } while (choice != 10);

    return 0;
}
```

---

## Input/Output

### Sample Run:

```
===== BST Non-Recursive Operations =====
Enter your choice: 9
Creating sample tree with values: 50 30 70 20 40 60 80 10 25 35 45
✓ Inserted 50
✓ Inserted 30
✓ Inserted 70
✓ Inserted 20
✓ Inserted 40
✓ Inserted 60
✓ Inserted 80
✓ Inserted 10
✓ Inserted 25
✓ Inserted 35
✓ Inserted 45

Enter your choice: 8

BST Structure:
└──50
    ├──30
    │   ├──20
    │   │   ├──10
    │   │   └──25
    │   └──40
    │       ├──35
    │       └──45
    └──70
        ├──60
        └──80

Enter your choice: 2
Inorder (Non-recursive): 10 20 25 30 35 40 45 50 60 70 80

Enter your choice: 3
Preorder (Non-recursive): 50 30 20 10 25 40 35 45 70 60 80

Enter your choice: 4
Level Order (Non-recursive): 50 30 70 20 40 60 80 10 25 35 45

Enter your choice: 5

Number of leaf nodes: 6

Enter your choice: 6

Total number of nodes: 11

Enter your choice: 7
✓ Mirror image created (Non-recursive)!

Enter your choice: 2
Inorder (Non-recursive): 80 70 60 50 45 40 35 30 25 20 10

Enter your choice: 10
Exiting... Thank you!
```

---

## Dry Run

### BST Structure with values: 50, 30, 70, 20, 40, 10, 25

```
Original BST:
      50
     /  \
   30    70
   / \   / \
 20  40 60 80
 / \
10 25
```

### Inorder Traversal (Non-recursive)

```
Stack: []
Current: 50

Step 1: Go to leftmost node
  Push 50, current = 30
  Push 30, current = 20
  Push 20, current = 10
  Push 10, current = NULL

Stack: [50, 30, 20, 10]

Step 2: Process nodes
  Pop 10, print 10, current = NULL
  Pop 20, print 20, current = 25
  Push 25, current = NULL
  Pop 25, print 25, current = NULL
  Pop 30, print 30, current = 40
  Push 40, current = NULL
  Pop 40, print 40, current = NULL
  Pop 50, print 50, current = 70
  Push 70, current = 60
  Push 60, current = NULL
  Pop 60, print 60, current = NULL
  Pop 70, print 70, current = 80
  Push 80, current = NULL
  Pop 80, print 80, current = NULL

Output: 10 20 25 30 40 50 60 70 80
```

### Preorder Traversal (Non-recursive)

```
Stack: [50]

Step 1: Pop 50, print 50
  Push 70 (right), then 30 (left)
  Stack: [70, 30]

Step 2: Pop 30, print 30
  Push 40 (right), then 20 (left)
  Stack: [70, 40, 20]

Step 3: Pop 20, print 20
  Push 25 (right), then 10 (left)
  Stack: [70, 40, 25, 10]

Step 4: Pop 10, print 10
  Stack: [70, 40, 25]

Step 5: Pop 25, print 25
  Stack: [70, 40]

Step 6: Pop 40, print 40
  Stack: [70]

Step 7: Pop 70, print 70
  Push 80 (right), then 60 (left)
  Stack: [80, 60]

Step 8: Pop 60, print 60
  Stack: [80]

Step 9: Pop 80, print 80
  Stack: []

Output: 50 30 20 10 25 40 70 60 80
```

### Count Leaf Nodes (Non-recursive)

```
Queue: [50]

Step 1: Dequeue 50
  Has children → enqueue 30, 70
  Queue: [30, 70]

Step 2: Dequeue 30
  Has children → enqueue 20, 40
  Queue: [70, 20, 40]

Step 3: Dequeue 70
  Has children → enqueue 60, 80
  Queue: [20, 40, 60, 80]

Step 4: Dequeue 20
  Has children → enqueue 10, 25
  Queue: [40, 60, 80, 10, 25]

Step 5: Dequeue 40
  Has children → enqueue 35, 45
  Queue: [60, 80, 10, 25, 35, 45]

Step 6: Dequeue 60
  No children → leafCount = 1
  Queue: [80, 10, 25, 35, 45]

Step 7: Dequeue 80
  No children → leafCount = 2
  Queue: [10, 25, 35, 45]

Step 8: Dequeue 10
  No children → leafCount = 3
  Queue: [25, 35, 45]

Step 9: Dequeue 25
  No children → leafCount = 4
  Queue: [35, 45]

Step 10: Dequeue 35
  No children → leafCount = 5
  Queue: [45]

Step 11: Dequeue 45
  No children → leafCount = 6
  Queue: []

Leaf nodes: 10, 25, 35, 45, 60, 80
Total leaf count: 6
```

### Mirror Image (Non-recursive)

```
Queue: [50]

Step 1: Dequeue 50
  Swap children: left=30, right=70 → left=70, right=30
  Enqueue 70, 30
  Queue: [70, 30]

Step 2: Dequeue 70
  Swap children: left=60, right=80 → left=80, right=60
  Enqueue 80, 60
  Queue: [30, 80, 60]

Step 3: Dequeue 30
  Swap children: left=20, right=40 → left=40, right=20
  Enqueue 40, 20
  Queue: [80, 60, 40, 20]

Step 4: Dequeue 80
  No children → no swap
  Queue: [60, 40, 20]

Step 5: Dequeue 60
  No children → no swap
  Queue: [40, 20]

Step 6: Dequeue 40
  Swap children: left=35, right=45 → left=45, right=35
  Enqueue 45, 35
  Queue: [20, 45, 35]

Step 7: Dequeue 20
  Swap children: left=10, right=25 → left=25, right=10
  Enqueue 25, 10
  Queue: [45, 35, 25, 10]

Continue until queue empty...

Final Mirror BST:
      50
     /  \
   70    30
   / \   / \
 80  60 40 20
        / \
       45 35
           / \
          25 10
```

---

## Conclusion

This program implements **Non-Recursive BST Operations** with the following achievements:

1. **Inorder Traversal (Non-recursive)**:

   - Uses stack to simulate recursion
   - Time complexity: O(n)
   - Space complexity: O(h) where h is height
   - Algorithm: Go to leftmost, process, move right

2. **Preorder Traversal (Non-recursive)**:

   - Uses stack with right-first push strategy
   - Time complexity: O(n)
   - Space complexity: O(h)
   - Algorithm: Process node, push right, push left

3. **Leaf Node Counting (Non-recursive)**:

   - Uses queue for level-order traversal
   - Time complexity: O(n)
   - Space complexity: O(w) where w is max width
   - Algorithm: Check if node has no children

4. **Mirror Image (Non-recursive)**:
   - Uses queue for level-order processing
   - Time complexity: O(n)
   - Space complexity: O(w)
   - Algorithm: Swap children at each level

**Key Non-Recursive Techniques**:

1. **Stack for DFS**: Simulates function call stack
2. **Queue for BFS**: Level-by-level processing
3. **Morris Traversal**: O(1) space (advanced)

**Time Complexity Comparison**:
| Operation | Recursive | Non-recursive |
|-----------|-----------|---------------|
| Inorder | O(n) | O(n) |
| Preorder | O(n) | O(n) |
| Leaf Count | O(n) | O(n) |
| Mirror | O(n) | O(n) |

**Space Complexity Comparison**:
| Operation | Recursive | Non-recursive |
|-----------|-----------|---------------|
| Inorder | O(h) | O(h) |
| Preorder | O(h) | O(h) |
| Leaf Count | O(h) | O(w) |
| Mirror | O(h) | O(w) |

**Advantages of Non-recursive Approach**:

- No stack overflow risk
- Better control over memory
- Easier to convert to iterative algorithms
- Suitable for systems with limited stack space

**Applications**:

- Embedded systems with limited stack
- Large tree processing
- Real-time systems
- Systems programming
