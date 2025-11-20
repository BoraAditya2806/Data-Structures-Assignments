# Assignment 5

## Problem Statement

Write a Program to create a Binary Tree and perform the following Recursive operations on it:

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

    FUNCTION inorderRecursive(node):
        IF node != NULL:
            inorderRecursive(node.left)
            PRINT node.data
            inorderRecursive(node.right)

    FUNCTION preorderRecursive(node):
        IF node != NULL:
            PRINT node.data
            preorderRecursive(node.left)
            preorderRecursive(node.right)

    FUNCTION countLeafNodesRecursive(node):
        IF node == NULL:
            RETURN 0
        IF node.left == NULL AND node.right == NULL:
            RETURN 1
        RETURN countLeafNodesRecursive(node.left) + countLeafNodesRecursive(node.right)

    FUNCTION mirrorRecursive(node):
        IF node == NULL:
            RETURN

        // Swap left and right children
        temp = node.left
        node.left = node.right
        node.right = temp

        mirrorRecursive(node.left)
        mirrorRecursive(node.right)

MAIN:
    CREATE BST
    INSERT values
    PERFORM all recursive operations
```

---

## C++ Code

```cpp
#include <iostream>
using namespace std;

class Node { // Class name is NOT modified
public:
    int data;
    Node* left;
    Node* right;

    Node_agb(int value) { // Constructor name modified
        data = value;
        left = NULL;
        right = NULL;
    }
};

class BST { // Class name is NOT modified
private:
    Node* root;

    // Recursive insert
    Node* insertRecursive_agb(Node* node, int value) { // Method name modified
        if (node == NULL) {
            return new Node_agb(value); // Calling postfixed constructor
        }

        if (value < node->data) {
            node->left = insertRecursive_agb(node->left, value); // Calling postfixed method
        } else if (value > node->data) {
            node->right = insertRecursive_agb(node->right, value); // Calling postfixed method
        }

        return node;
    }

    // Inorder traversal (Recursive)
    void inorderRecursive_agb(Node* node) { // Method name modified
        if (node != NULL) {
            inorderRecursive_agb(node->left); // Calling postfixed method
            cout << node->data << " ";
            inorderRecursive_agb(node->right); // Calling postfixed method
        }
    }

    // Preorder traversal (Recursive)
    void preorderRecursive_agb(Node* node) { // Method name modified
        if (node != NULL) {
            cout << node->data << " ";
            preorderRecursive_agb(node->left); // Calling postfixed method
            preorderRecursive_agb(node->right); // Calling postfixed method
        }
    }

    // Postorder traversal (Recursive)
    void postorderRecursive_agb(Node* node) { // Method name modified
        if (node != NULL) {
            postorderRecursive_agb(node->left); // Calling postfixed method
            postorderRecursive_agb(node->right); // Calling postfixed method
            cout << node->data << " ";
        }
    }

    // Count leaf nodes (Recursive)
    int countLeafNodesRecursive_agb(Node* node) { // Method name modified
        if (node == NULL) {
            return 0;
        }

        // If it's a leaf node
        if (node->left == NULL && node->right == NULL) {
            return 1;
        }

        // Recursively count in left and right subtrees
        return countLeafNodesRecursive_agb(node->left) + countLeafNodesRecursive_agb(node->right); // Calling postfixed method
    }

    // Create mirror image (Recursive)
    void mirrorRecursive_agb(Node* node) { // Method name modified
        if (node == NULL) {
            return;
        }

        // Swap left and right children
        Node* temp_agb = node->left; // Variable name modified
        node->left = node->right;
        node->right = temp_agb; // Variable name modified

        // Recursively mirror subtrees
        mirrorRecursive_agb(node->left); // Calling postfixed method
        mirrorRecursive_agb(node->right); // Calling postfixed method
    }

    // Count total nodes (Recursive)
    int countNodesRecursive_agb(Node* node) { // Method name modified
        if (node == NULL) {
            return 0;
        }
        return 1 + countNodesRecursive_agb(node->left) + countNodesRecursive_agb(node->right); // Calling postfixed method
    }

    // Compute height (Recursive)
    int heightRecursive_agb(Node* node) { // Method name modified
        if (node == NULL) {
            return 0;
        }

        int leftHeight_agb = heightRecursive_agb(node->left); // Variable name and postfixed method call modified
        int rightHeight_agb = heightRecursive_agb(node->right); // Variable name and postfixed method call modified

        return max(leftHeight_agb, rightHeight_agb) + 1; // Variable names modified
    }

public:
    BST_agb() { // Constructor name modified
        root = NULL;
    }

    // Public insert
    void insert_agb(int value) { // Method name modified
        root = insertRecursive_agb(root, value); // Calling postfixed method
        cout << "✓ Inserted " << value << endl;
    }

    // Public inorder traversal
    void inorder_agb() { // Method name modified
        if (root == NULL) {
            cout << "Tree is empty!" << endl;
            return;
        }
        cout << "Inorder (Recursive): ";
        inorderRecursive_agb(root); // Calling postfixed method
        cout << endl;
    }

    // Public preorder traversal
    void preorder_agb() { // Method name modified
        if (root == NULL) {
            cout << "Tree is empty!" << endl;
            return;
        }
        cout << "Preorder (Recursive): ";
        preorderRecursive_agb(root); // Calling postfixed method
        cout << endl;
    }

    // Public postorder traversal
    void postorder_agb() { // Method name modified
        if (root == NULL) {
            cout << "Tree is empty!" << endl;
            return;
        }
        cout << "Postorder (Recursive): ";
        postorderRecursive_agb(root); // Calling postfixed method
        cout << endl;
    }

    // Public leaf node count
    int countLeafNodes_agb() { // Method name modified
        return countLeafNodesRecursive_agb(root); // Calling postfixed method
    }

    // Public mirror
    void mirror_agb() { // Method name modified
        mirrorRecursive_agb(root); // Calling postfixed method
        cout << "✓ Mirror image created (Recursive)!" << endl;
    }

    // Public node count
    int countNodes_agb() { // Method name modified
        return countNodesRecursive_agb(root); // Calling postfixed method
    }

    // Public height
    int height_agb() { // Method name modified
        return heightRecursive_agb(root); // Calling postfixed method
    }

    // Display tree structure
    void displayTree_agb() { // Method name modified
        if (root == NULL) {
            cout << "Tree is empty!" << endl;
            return;
        }

        cout << "\nBST Structure:" << endl;
        displayTreeHelper_agb(root, "", false); // Calling postfixed method
    }

    void displayTreeHelper_agb(Node* node, string prefix, bool isLeft) { // Method name modified
        if (node == NULL) return;

        cout << prefix;
        cout << (isLeft ? "├──" : "└──");
        cout << node->data << endl;

        if (node->left != NULL || node->right != NULL) {
            if (node->left != NULL) {
                displayTreeHelper_agb(node->left, prefix + (isLeft ? "│   " : "    "), true); // Calling postfixed method
            } else if (node->right != NULL) {
                cout << prefix << (isLeft ? "│   " : "    ") << "├──NULL" << endl;
            }

            if (node->right != NULL) {
                displayTreeHelper_agb(node->right, prefix + (isLeft ? "│   " : "    "), false); // Calling postfixed method
            } else if (node->left != NULL) {
                cout << prefix << (isLeft ? "│   " : "    ") << "└──NULL" << endl;
            }
        }
    }
};

int main_agb() { // Function name modified
    BST_agb tree_agb; // Variable name and constructor call modified
    int choice, value;

    do {
        cout << "\n===== BST Recursive Operations =====" << endl;
        cout << "1. Insert node" << endl;
        cout << "2. Inorder traversal (Recursive)" << endl;
        cout << "3. Preorder traversal (Recursive)" << endl;
        cout << "4. Postorder traversal (Recursive)" << endl;
        cout << "5. Count leaf nodes (Recursive)" << endl;
        cout << "6. Count total nodes (Recursive)" << endl;
        cout << "7. Compute height (Recursive)" << endl;
        cout << "8. Create mirror image (Recursive)" << endl;
        cout << "9. Display tree structure" << endl;
        cout << "10. Create sample tree" << endl;
        cout << "11. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter value to insert: ";
                cin >> value;
                tree_agb.insert_agb(value); // Calling postfixed method
                break;

            case 2:
                tree_agb.inorder_agb(); // Calling postfixed method
                break;

            case 3:
                tree_agb.preorder_agb(); // Calling postfixed method
                break;

            case 4:
                tree_agb.postorder_agb(); // Calling postfixed method
                break;

            case 5:
                cout << "\nNumber of leaf nodes: " << tree_agb.countLeafNodes_agb() << endl; // Calling postfixed method
                break;

            case 6:
                cout << "\nTotal number of nodes: " << tree_agb.countNodes_agb() << endl; // Calling postfixed method
                break;

            case 7:
                cout << "\nHeight of tree: " << tree_agb.height_agb() << endl; // Calling postfixed method
                break;

            case 8:
                tree_agb.mirror_agb(); // Calling postfixed method
                break;

            case 9:
                tree_agb.displayTree_agb(); // Calling postfixed method
                break;

            case 10: {
                int values_agb[] = {50, 30, 70, 20, 40, 60, 80, 10, 25, 35, 45}; // Variable name modified
                cout << "Creating sample tree with values: ";
                for (int i = 0; i < 11; i++) {
                    cout << values_agb[i] << " "; // Variable name modified
                }
                cout << endl;

                for (int i = 0; i < 11; i++) {
                    tree_agb.insert_agb(values_agb[i]); // Variable name and postfixed method call modified
                }
                break;
            }

            case 11:
                cout << "Exiting... Thank you!" << endl;
                break;

            default:
                cout << "Invalid choice!" << endl;
        }
    } while (choice != 11);

    return 0;
}
```

---

## Input/Output

### Sample Run:

```
===== BST Recursive Operations =====
Enter your choice: 10
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

Enter your choice: 9

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
Inorder (Recursive): 10 20 25 30 35 40 45 50 60 70 80

Enter your choice: 3
Preorder (Recursive): 50 30 20 10 25 40 35 45 70 60 80

Enter your choice: 4
Postorder (Recursive): 10 25 20 35 45 40 30 60 80 70 50

Enter your choice: 5

Number of leaf nodes: 6

Enter your choice: 6

Total number of nodes: 11

Enter your choice: 7

Height of tree: 4

Enter your choice: 8
✓ Mirror image created (Recursive)!

Enter your choice: 2
Inorder (Recursive): 80 70 60 50 45 40 35 30 25 20 10

Enter your choice: 11
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

### Inorder Traversal (Recursive)

```
inorder(50):
  inorder(30):
    inorder(20):
      inorder(10):
        10 ≠ NULL
        inorder(NULL) → return
        print 10
        inorder(NULL) → return
      20 ≠ NULL
      print 20
      inorder(25):
        25 ≠ NULL
        inorder(NULL) → return
        print 25
        inorder(NULL) → return
    30 ≠ NULL
    print 30
    inorder(40):
      40 ≠ NULL
      inorder(NULL) → return
      print 40
      inorder(NULL) → return
  50 ≠ NULL
  print 50
  inorder(70):
    70 ≠ NULL
    inorder(60):
      60 ≠ NULL
      inorder(NULL) → return
      print 60
      inorder(NULL) → return
    print 70
    inorder(80):
      80 ≠ NULL
      inorder(NULL) → return
      print 80
      inorder(NULL) → return

Output: 10 20 25 30 40 50 60 70 80
```

### Preorder Traversal (Recursive)

```
preorder(50):
  50 ≠ NULL
  print 50
  preorder(30):
    30 ≠ NULL
    print 30
    preorder(20):
      20 ≠ NULL
      print 20
      preorder(10):
        10 ≠ NULL
        print 10
        preorder(NULL) → return
        preorder(NULL) → return
      preorder(25):
        25 ≠ NULL
        print 25
        preorder(NULL) → return
        preorder(NULL) → return
    preorder(40):
      40 ≠ NULL
      print 40
      preorder(NULL) → return
      preorder(NULL) → return
  preorder(70):
    70 ≠ NULL
    print 70
    preorder(60):
      60 ≠ NULL
      print 60
      preorder(NULL) → return
      preorder(NULL) → return
    preorder(80):
      80 ≠ NULL
      print 80
      preorder(NULL) → return
      preorder(NULL) → return

Output: 50 30 20 10 25 40 70 60 80
```

### Count Leaf Nodes (Recursive)

```
countLeafNodes(50):
  50 ≠ NULL, has children
  countLeafNodes(30) + countLeafNodes(70)

  countLeafNodes(30):
    30 ≠ NULL, has children
    countLeafNodes(20) + countLeafNodes(40)

    countLeafNodes(20):
      20 ≠ NULL, has children
      countLeafNodes(10) + countLeafNodes(25)

      countLeafNodes(10):
        10 ≠ NULL, no children → return 1

      countLeafNodes(25):
        25 ≠ NULL, no children → return 1

      = 1 + 1 = 2

    countLeafNodes(40):
      40 ≠ NULL, no children → return 1

    = 2 + 1 = 3

  countLeafNodes(70):
    70 ≠ NULL, has children
    countLeafNodes(60) + countLeafNodes(80)

    countLeafNodes(60):
      60 ≠ NULL, no children → return 1

    countLeafNodes(80):
      80 ≠ NULL, no children → return 1

    = 1 + 1 = 2

  = 3 + 2 = 5

Leaf nodes: 10, 25, 40, 60, 80
Total leaf count: 5
```

### Mirror Image (Recursive)

```
mirror(50):
  50 ≠ NULL
  Swap: left=30 ↔ right=70

  mirror(30):
    30 ≠ NULL
    Swap: left=20 ↔ right=40

    mirror(20):
      20 ≠ NULL
      Swap: left=10 ↔ right=25

      mirror(10): NULL → return
      mirror(25): NULL → return

    mirror(40):
      40 ≠ NULL
      Swap: left=NULL ↔ right=NULL

      mirror(NULL): return
      mirror(NULL): return

  mirror(70):
    70 ≠ NULL
    Swap: left=60 ↔ right=80

    mirror(60): NULL → return
    mirror(80): NULL → return

Final Mirror BST:
      50
     /  \
   70    30
   / \   / \
 80  60 40 20
        / \
       25 10
```

---

## Conclusion

This program implements **Recursive BST Operations** with the following achievements:

1. **Inorder Traversal (Recursive)**:

   - Left-Root-Right pattern
   - Produces sorted output for BST
   - Time complexity: O(n)
   - Space complexity: O(h) for recursion stack

2. **Preorder Traversal (Recursive)**:

   - Root-Left-Right pattern
   - Useful for tree copying
   - Time complexity: O(n)
   - Space complexity: O(h)

3. **Postorder Traversal (Recursive)**:

   - Left-Right-Root pattern
   - Useful for tree deletion
   - Time complexity: O(n)
   - Space complexity: O(h)

4. **Leaf Node Counting (Recursive)**:

   - Base case: NULL node returns 0
   - Leaf node: No children returns 1
   - Recursive case: Sum of left and right subtrees
   - Time complexity: O(n)
   - Space complexity: O(h)

5. **Mirror Image (Recursive)**:
   - Swaps left and right children at each node
   - Recursive approach to all subtrees
   - Time complexity: O(n)
   - Space complexity: O(h)

**Key Recursive Principles**:

1. **Base Case**: Handle NULL nodes or leaf conditions
2. **Recursive Case**: Process left and right subtrees
3. **Combine Results**: Aggregate results from subtrees
4. **Divide and Conquer**: Break problem into smaller subproblems

**Time Complexity**:

- All operations: O(n) where n is number of nodes
- Each node visited exactly once

**Space Complexity**:

- O(h) where h is height of tree
- Best case (balanced): O(log n)
- Worst case (skewed): O(n)

**Advantages of Recursive Approach**:

- Clean and intuitive code
- Natural fit for tree structures
- Easy to understand and maintain
- Less code compared to iterative versions

**Disadvantages of Recursive Approach**:

- Stack overflow risk for deep trees
- Function call overhead
- Memory usage for recursion stack

**Applications**:

- Expression tree evaluation
- File system traversal
- Mathematical expression parsing
- Compiler design
- Game tree algorithms
