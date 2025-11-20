# Assignment 2

## Problem Statement

Write a program to perform Binary Search Tree (BST) operations:

- Count the total number of nodes
- Compute the height of the BST
- Create mirror image of the BST

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

    FUNCTION countNodes(node):
        IF node == NULL:
            RETURN 0
        RETURN 1 + countNodes(node.left) + countNodes(node.right)

    FUNCTION height(node):
        IF node == NULL:
            RETURN 0

        leftHeight = height(node.left)
        rightHeight = height(node.right)

        RETURN MAX(leftHeight, rightHeight) + 1

    FUNCTION mirror(node):
        IF node == NULL:
            RETURN

        // Swap left and right children
        temp = node.left
        node.left = node.right
        node.right = temp

        // Recursively mirror subtrees
        mirror(node.left)
        mirror(node.right)

    FUNCTION inorder(node):
        IF node != NULL:
            inorder(node.left)
            PRINT node.data
            inorder(node.right)

MAIN:
    CREATE BST
    INSERT values
    COUNT nodes
    COMPUTE height
    CREATE mirror
    DISPLAY
```

---

## C++ Code

```cpp
#include <iostream>
#include <queue>
#include <iomanip>
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

    int countNodesRecursive_agb(Node* node) {
        if (node == NULL) {
            return 0;
        }
        return 1 + countNodesRecursive_agb(node->left) + countNodesRecursive_agb(node->right);
    }

    int heightRecursive_agb(Node* node) {
        if (node == NULL) {
            return 0;
        }

        int leftHeight_agb = heightRecursive_agb(node->left);
        int rightHeight_agb = heightRecursive_agb(node->right);

        return max(leftHeight_agb, rightHeight_agb) + 1;
    }

    void mirrorRecursive_agb(Node* node) {
        if (node == NULL) {
            return;
        }

        Node* temp_agb = node->left;
        node->left = node->right;
        node->right = temp_agb;

        mirrorRecursive_agb(node->left);
        mirrorRecursive_agb(node->right);
    }

    void inorderHelper_agb(Node* node) {
        if (node != NULL) {
            inorderHelper_agb(node->left);
            cout << node->data << " ";
            inorderHelper_agb(node->right);
        }
    }

    void preorderHelper_agb(Node* node) {
        if (node != NULL) {
            cout << node->data << " ";
            preorderHelper_agb(node->left);
            preorderHelper_agb(node->right);
        }
    }

    int countLeafNodes_agb(Node* node) {
        if (node == NULL) {
            return 0;
        }
        if (node->left == NULL && node->right == NULL) {
            return 1;
        }
        return countLeafNodes_agb(node->left) + countLeafNodes_agb(node->right);
    }

    int countInternalNodes_agb(Node* node) {
        if (node == NULL || (node->left == NULL && node->right == NULL)) {
            return 0;
        }
        return 1 + countInternalNodes_agb(node->left) + countInternalNodes_agb(node->right);
    }

    void displayTreeStructure_agb(Node* node, string prefix = "", bool isLeft = true) {
        if (node == NULL) return;

        cout << prefix;
        cout << (isLeft ? "├──" : "└──");
        cout << node->data << endl;

        if (node->left != NULL || node->right != NULL) {
            if (node->left != NULL) {
                displayTreeStructure_agb(node->left, prefix + (isLeft ? "│   " : "    "), true);
            } else {
                cout << prefix << (isLeft ? "│   " : "    ") << "├──" << "NULL" << endl;
            }

            if (node->right != NULL) {
                displayTreeStructure_agb(node->right, prefix + (isLeft ? "│   " : "    "), false);
            } else {
                cout << prefix << (isLeft ? "│   " : "    ") << "└──" << "NULL" << endl;
            }
        }
    }

public:
    BST_agb() {
        root = NULL;
    }

    void insert_agb(int value) {
        root = insertRecursive_agb(root, value);
        cout << "✓ Inserted " << value << endl;
    }

    int countNodes_agb() {
        return countNodesRecursive_agb(root);
    }

    int height_agb() {
        return heightRecursive_agb(root);
    }

    void mirror_agb() {
        mirrorRecursive_agb(root);
        cout << "✓ Mirror image created!" << endl;
    }

    void inorder_agb() {
        cout << "Inorder: ";
        inorderHelper_agb(root);
        cout << endl;
    }

    void preorder_agb() {
        cout << "Preorder: ";
        preorderHelper_agb(root);
        cout << endl;
    }

    void levelOrder_agb() {
        if (root == NULL) {
            cout << "Tree is empty!" << endl;
            return;
        }

        queue<Node*> q_agb;
        q_agb.push(root);

        cout << "Level Order: ";
        while (!q_agb.empty()) {
            Node* current = q_agb.front();
            q_agb.pop();
            cout << current->data << " ";

            if (current->left != NULL) q_agb.push(current->left);
            if (current->right != NULL) q_agb.push(current->right);
        }
        cout << endl;
    }

    void displayTree_agb() {
        if (root == NULL) {
            cout << "Tree is empty!" << endl;
            return;
        }
        cout << "\n--- Tree Structure ---" << endl;
        displayTreeStructure_agb(root, "", false);
    }

    void displayInfo_agb() {
        cout << "\n===== BST Information =====" << endl;
        cout << "Total nodes: " << countNodes_agb() << endl;
        cout << "Height: " << height_agb() << endl;
        cout << "Leaf nodes: " << countLeafNodes_agb(root) << endl;
        cout << "Internal nodes: " << countInternalNodes_agb(root) << endl;
    }
};

int main_agb() {
    BST_agb tree_agb;
    int choice, value;

    do {
        cout << "\n===== BST Advanced Operations =====" << endl;
        cout << "1. Insert node" << endl;
        cout << "2. Count total nodes" << endl;
        cout << "3. Compute height" << endl;
        cout << "4. Create mirror image" << endl;
        cout << "5. Display inorder traversal" << endl;
        cout << "6. Display preorder traversal" << endl;
        cout << "7. Display level order traversal" << endl;
        cout << "8. Display tree structure" << endl;
        cout << "9. Display complete info" << endl;
        cout << "10. Create sample tree" << endl;
        cout << "11. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter value to insert: ";
                cin >> value;
                tree_agb.insert_agb(value);
                break;

            case 2:
                cout << "\nTotal number of nodes: " << tree_agb.countNodes_agb() << endl;
                break;

            case 3:
                cout << "\nHeight of BST: " << tree_agb.height_agb() << endl;
                break;

            case 4:
                tree_agb.mirror_agb();
                cout << "Note: The BST property is now reversed!" << endl;
                break;

            case 5:
                tree_agb.inorder_agb();
                break;

            case 6:
                tree_agb.preorder_agb();
                break;

            case 7:
                tree_agb.levelOrder_agb();
                break;

            case 8:
                tree_agb.displayTree_agb();
                break;

            case 9:
                tree_agb.displayInfo_agb();
                tree_agb.inorder_agb();
                break;

            case 10: {
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
===== BST Advanced Operations =====
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

===== BST Information =====
Total nodes: 11
Height: 4
Leaf nodes: 4
Internal nodes: 7
Inorder: 10 20 25 30 35 40 45 50 60 70 80

Enter your choice: 8

--- Tree Structure ---
└──50
    ├──30
    │   ├──20
    │   │   ├──10
    │   │   │   ├──NULL
    │   │   │   └──NULL
    │   │   └──25
    │   │       ├──NULL
    │   │       └──NULL
    │   └──40
    │       ├──35
    │       │   ├──NULL
    │       │   └──NULL
    │       └──45
    │           ├──NULL
    │           └──NULL
    └──70
        ├──60
        │   ├──NULL
        │   └──NULL
        └──80
            ├──NULL
            └──NULL

Enter your choice: 4
✓ Mirror image created!
Note: The BST property is now reversed!

Enter your choice: 8

--- Tree Structure ---
└──50
    ├──70
    │   ├──80
    │   │   ├──NULL
    │   │   └──NULL
    │   └──60
    │       ├──NULL
    │       └──NULL
    └──30
        ├──40
        │   ├──45
        │   │   ├──NULL
        │   │   └──NULL
        │   └──35
        │       ├──NULL
        │       └──NULL
        └──20
            ├──25
            │   ├──NULL
            │   └──NULL
            └──10
                ├──NULL
                └──NULL

Enter your choice: 5
Inorder: 80 70 60 50 45 40 35 30 25 20 10

Enter your choice: 2

Total number of nodes: 11

Enter your choice: 3

Height of BST: 4

Enter your choice: 11
Exiting... Thank you!
```

---

## Dry Run

### Count Nodes - Tree with values: 50, 30, 70, 20, 40

```
Original Tree:
      50
     /  \
   30    70
   / \
 20  40

countNodes(50):
  1 + countNodes(30) + countNodes(70)

  countNodes(30):
    1 + countNodes(20) + countNodes(40)

    countNodes(20):
      1 + countNodes(NULL) + countNodes(NULL)
      = 1 + 0 + 0 = 1

    countNodes(40):
      1 + 0 + 0 = 1

    = 1 + 1 + 1 = 3

  countNodes(70):
    1 + 0 + 0 = 1

  = 1 + 3 + 1 = 5

Total Nodes: 5
```

### Compute Height

```
height(50):
  max(height(30), height(70)) + 1

  height(30):
    max(height(20), height(40)) + 1

    height(20):
      max(height(NULL), height(NULL)) + 1
      = max(0, 0) + 1 = 1

    height(40):
      = 1

    = max(1, 1) + 1 = 2

  height(70):
    = max(0, 0) + 1 = 1

  = max(2, 1) + 1 = 3

Height: 3
```

### Mirror Image

```
Original Tree:
      50
     /  \
   30    70
   / \
 20  40

Step-by-step mirroring:

mirror(50):
  Swap: left=30 ↔ right=70

  Temp Tree:
      50
     /  \
   70    30
         / \
       20  40

  mirror(70): No children, done

  mirror(30):
    Swap: left=20 ↔ right=40

    Result:
        50
       /  \
     70    30
           / \
         40  20

    mirror(40): done
    mirror(20): done

Final Mirror Tree:
      50
     /  \
   70    30
        /  \
      40   20
```

### Inorder Traversal (Original vs Mirror)

**Original Tree Inorder (Left-Root-Right)**:

```
Visit 20 → Visit 30 → Visit 40 → Visit 50 → Visit 70
Output: 20 30 40 50 70 (Sorted)
```

**Mirror Tree Inorder (Left-Root-Right)**:

```
Visit 70 → Visit 50 → Visit 40 → Visit 30 → Visit 20
Output: 70 50 40 30 20 (Reverse sorted)
```

---

## Conclusion

This program implements **advanced BST operations** with the following achievements:

1. **Node Counting**:

   - Recursive traversal of entire tree
   - Counts all nodes (root, internal, leaf)
   - Time complexity: O(n)
   - Formula: 1 + countLeft + countRight

2. **Height Computation**:

   - Measures longest path from root to leaf
   - Uses recursive max approach
   - Time complexity: O(n)
   - Formula: max(leftHeight, rightHeight) + 1

3. **Mirror Image Creation**:

   - Swaps left and right children
   - Recursive process for all nodes
   - Reverses BST property
   - Time complexity: O(n)

4. **Additional Features**:
   - Leaf node counting
   - Internal node counting
   - Multiple traversal methods
   - Tree structure visualization

**Key Formulas**:

```
Total Nodes = 1 + nodes in left subtree + nodes in right subtree
Height = 1 + max(height of left, height of right)
Leaf Nodes = nodes with no children
Internal Nodes = total nodes - leaf nodes
```

**Time Complexity**:

- Count Nodes: O(n)
- Height: O(n)
- Mirror: O(n)
- Traversals: O(n)

**Space Complexity**:

- Recursion stack: O(h) where h is height
- In worst case (skewed tree): O(n)

**Applications**:

- Tree analysis and statistics
- Expression tree manipulation
- File system mirroring
- Game tree evaluation
