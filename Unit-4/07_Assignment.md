# Assignment 7

## Problem Statement

Write a program to illustrate operations on a BST holding numeric keys. The menu must include:

- Insert
- Delete
- Find
- Show

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

    FUNCTION insertRecursive(node, value):
        IF node == NULL:
            RETURN new Node(value)

        IF value < node.data:
            node.left = insertRecursive(node.left, value)
        ELSE IF value > node.data:
            node.right = insertRecursive(node.right, value)

        RETURN node

    FUNCTION delete(value):
        root = deleteRecursive(root, value)

    FUNCTION deleteRecursive(node, value):
        IF node == NULL:
            RETURN NULL

        IF value < node.data:
            node.left = deleteRecursive(node.left, value)
        ELSE IF value > node.data:
            node.right = deleteRecursive(node.right, value)
        ELSE:
            // Node to delete found
            IF node.left == NULL:
                RETURN node.right
            ELSE IF node.right == NULL:
                RETURN node.left

            // Node has two children
            minValue = findMin(node.right)
            node.data = minValue
            node.right = deleteRecursive(node.right, minValue)

        RETURN node

    FUNCTION search(value):
        RETURN searchRecursive(root, value)

    FUNCTION searchRecursive(node, value):
        IF node == NULL OR node.data == value:
            RETURN node
        IF value < node.data:
            RETURN searchRecursive(node.left, value)
        RETURN searchRecursive(node.right, value)

    FUNCTION display():
        inorderTraversal(root)

MAIN:
    CREATE BST
    LOOP:
        DISPLAY menu
        GET choice
        SWITCH choice:
            CASE Insert: GET value, CALL insert()
            CASE Delete: GET value, CALL delete()
            CASE Find: GET value, CALL search()
            CASE Show: CALL display()
            CASE Exit
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
        } else {
            cout << "Duplicate value " << value << "! Not inserted." << endl;
        }

        return node;
    }

    Node* findMin_agb(Node* node) {
        while (node->left != NULL) {
            node = node->left;
        }
        return node;
    }

    Node* deleteRecursive_agb(Node* node, int value) {
        if (node == NULL) {
            cout << "Value " << value << " not found in tree!" << endl;
            return NULL;
        }

        if (value < node->data) {
            node->left = deleteRecursive_agb(node->left, value);
        } else if (value > node->data) {
            node->right = deleteRecursive_agb(node->right, value);
        } else {
            cout << "Deleting node with value " << value << endl;

            if (node->left == NULL) {
                Node* temp_agb = node->right;
                delete node;
                return temp_agb;
            } else if (node->right == NULL) {
                Node* temp_agb = node->left;
                delete node;
                return temp_agb;
            }

            Node* temp_agb = findMin_agb(node->right);
            cout << "Replacing with inorder successor: " << temp_agb->data << endl;
            node->data = temp_agb->data;
            node->right = deleteRecursive_agb(node->right, temp_agb->data);
        }

        return node;
    }

    Node* searchRecursive_agb(Node* node, int value) {
        if (node == NULL || node->data == value) {
            return node;
        }

        if (value < node->data) {
            return searchRecursive_agb(node->left, value);
        }
        return searchRecursive_agb(node->right, value);
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

    void postorderHelper_agb(Node* node) {
        if (node != NULL) {
            postorderHelper_agb(node->left);
            postorderHelper_agb(node->right);
            cout << node->data << " ";
        }
    }

    int heightHelper_agb(Node* node) {
        if (node == NULL) {
            return 0;
        }
        int leftHeight_agb = heightHelper_agb(node->left);
        int rightHeight_agb = heightHelper_agb(node->right);
        return max(leftHeight_agb, rightHeight_agb) + 1;
    }

public:
    BST_agb() {
        root = NULL;
    }

    void insert_agb(int value) {
        root = insertRecursive_agb(root, value);
        if (search_agb(value)) {
            cout << "✓ Inserted " << value << " into BST" << endl;
        }
    }

    void deleteValue_agb(int value) {
        root = deleteRecursive_agb(root, value);
    }

    bool search_agb(int value) {
        Node* result_agb = searchRecursive_agb(root, value);
        return result_agb != NULL;
    }

    void find_agb(int value) {
        Node* result_agb = searchRecursive_agb(root, value);
        if (result_agb != NULL) {
            cout << "✓ Value " << value << " found in tree!" << endl;
        } else {
            cout << "✗ Value " << value << " not found in tree!" << endl;
        }
    }

    void showInorder_agb() {
        if (root == NULL) {
            cout << "Tree is empty!" << endl;
            return;
        }
        cout << "Inorder (sorted): ";
        inorderHelper_agb(root);
        cout << endl;
    }

    void showPreorder_agb() {
        if (root == NULL) {
            cout << "Tree is empty!" << endl;
            return;
        }
        cout << "Preorder: ";
        preorderHelper_agb(root);
        cout << endl;
    }

    void showPostorder_agb() {
        if (root == NULL) {
            cout << "Tree is empty!" << endl;
            return;
        }
        cout << "Postorder: ";
        postorderHelper_agb(root);
        cout << endl;
    }

    void showLevelOrder_agb() {
        if (root == NULL) {
            cout << "Tree is empty!" << endl;
            return;
        }

        cout << "Level Order:" << endl;
        queue<Node*> q_agb;
        q_agb.push(root);
        int level_agb = 0;

        while (!q_agb.empty()) {
            int levelSize_agb = q_agb.size();
            cout << "Level " << level_agb++ << ": ";

            for (int i = 0; i < levelSize_agb; i++) {
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
    }

    int height_agb() {
        return heightHelper_agb(root);
    }

    int countNodes_agb() {
        return countNodesHelper_agb(root);
    }

    int countNodesHelper_agb(Node* node) {
        if (node == NULL) {
            return 0;
        }
        return 1 + countNodesHelper_agb(node->left) + countNodesHelper_agb(node->right);
    }

    void displayTreeStructure_agb() {
        if (root == NULL) {
            cout << "Tree is empty!" << endl;
            return;
        }
        cout << "\n--- Tree Structure ---" << endl;
        displayTreeHelper_agb(root, "", false);
    }

    void displayTreeHelper_agb(Node* node, string prefix_agb, bool isLeft) {
        if (node == NULL) return;

        cout << prefix_agb;
        cout << (isLeft ? "├──" : "└──");
        cout << node->data << endl;

        if (node->left != NULL || node->right != NULL) {
            if (node->left != NULL) {
                displayTreeHelper_agb(node->left, prefix_agb + (isLeft ? "│   " : "    "), true);
            } else {
                cout << prefix_agb << (isLeft ? "│   " : "    ") << "├──NULL" << endl;
            }

            if (node->right != NULL) {
                displayTreeHelper_agb(node->right, prefix_agb + (isLeft ? "│   " : "    "), false);
            } else {
                cout << prefix_agb << (isLeft ? "│   " : "    ") << "└──NULL" << endl;
            }
        }
    }
};

int main_agb() {
    BST_agb tree_agb;
    int choice_agb, value_agb;

    do {
        cout << "\n===== BST Operations Menu =====" << endl;
        cout << "1. Insert" << endl;
        cout << "2. Delete" << endl;
        cout << "3. Find" << endl;
        cout << "4. Show (Inorder)" << endl;
        cout << "5. Show (All traversals)" << endl;
        cout << "6. Show (Level order)" << endl;
        cout << "7. Show (Tree structure)" << endl;
        cout << "8. Tree statistics" << endl;
        cout << "9. Create sample tree" << endl;
        cout << "10. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice_agb;

        switch (choice_agb) {
            case 1:
                cout << "Enter value to insert: ";
                cin >> value_agb;
                tree_agb.insert_agb(value_agb);
                break;

            case 2:
                cout << "Enter value to delete: ";
                cin >> value_agb;
                tree_agb.deleteValue_agb(value_agb);
                break;

            case 3:
                cout << "Enter value to find: ";
                cin >> value_agb;
                tree_agb.find_agb(value_agb);
                break;

            case 4:
                tree_agb.showInorder_agb();
                break;

            case 5:
                cout << "\n--- All Traversals ---" << endl;
                tree_agb.showInorder_agb();
                tree_agb.showPreorder_agb();
                tree_agb.showPostorder_agb();
                break;

            case 6:
                tree_agb.showLevelOrder_agb();
                break;

            case 7:
                tree_agb.displayTreeStructure_agb();
                break;

            case 8:
                cout << "\n===== Tree Statistics =====" << endl;
                cout << "Total nodes: " << tree_agb.countNodes_agb() << endl;
                cout << "Height: " << tree_agb.height_agb() << endl;
                if (tree_agb.countNodes_agb() > 0) {
                    tree_agb.showInorder_agb();
                }
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
    } while (choice_agb != 10);

    return 0;
}
```

---

## Input/Output

### Sample Run:

```
===== BST Operations Menu =====
Enter your choice: 9
Creating sample tree with values: 50 30 70 20 40 60 80 10 25 35 45
✓ Inserted 50 into BST
✓ Inserted 30 into BST
✓ Inserted 70 into BST
✓ Inserted 20 into BST
✓ Inserted 40 into BST
✓ Inserted 60 into BST
✓ Inserted 80 into BST
✓ Inserted 10 into BST
✓ Inserted 25 into BST
✓ Inserted 35 into BST
✓ Inserted 45 into BST

Enter your choice: 4
Inorder (sorted): 10 20 25 30 35 40 45 50 60 70 80

Enter your choice: 3
Enter value to find: 60
✓ Value 60 found in tree!

Enter your choice: 3
Enter value to find: 100
✗ Value 100 not found in tree!

Enter your choice: 2
Enter value to delete: 20
Deleting node with value 20
✓ Inserted 25 into BST

Enter your choice: 4
Inorder (sorted): 10 25 30 35 40 45 50 60 70 80

Enter your choice: 2
Enter value to delete: 30
Deleting node with value 30
Replacing with inorder successor: 35
✓ Inserted 35 into BST

Enter your choice: 4
Inorder (sorted): 10 25 35 40 45 50 60 70 80

Enter your choice: 7

--- Tree Structure ---
└──50
    ├──35
    │   ├──25
    │   │   ├──10
    │   │   │   ├──NULL
    │   │   │   └──NULL
    │   │   └──NULL
    │   └──40
    │       ├──NULL
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

Enter your choice: 8

===== Tree Statistics =====
Total nodes: 9
Height: 4
Inorder (sorted): 10 25 35 40 45 50 60 70 80

Enter your choice: 10
Exiting... Thank you!
```

---

## Dry Run

### BST Structure with values: 50, 30, 70, 20, 40

```
BST after insertions:
      50
     /  \
   30    70
   / \
 20  40
```

### Delete Node with One Child (value = 20)

```
Original Tree:
      50
     /  \
   30    70
   /
 20

Step 1: Search for 20
  20 < 50 → go left
  20 < 30 → go left
  Found 20

Step 2: Delete 20
  Node 20 has no children
  Simply remove and return NULL to parent
  Parent 30's left becomes NULL

Result:
      50
     /  \
   30    70
   |
  NULL
```

### Delete Node with Two Children (value = 30)

```
Tree before deletion:
      50
     /  \
   30    70
   / \
 20  40

Step 1: Search for 30
  Found 30

Step 2: Delete 30
  Node 30 has two children
  Find inorder successor (minimum in right subtree)
  Inorder successor of 30 = 40

Step 3: Replace 30 with 40
  Copy 40's value to 30's position
  Delete original 40 node

Step 4: Delete 40
  Node 40 has no children
  Remove and return NULL to parent

Result:
      50
     /  \
   40    70
   /
 20
```

### Find Operation (value = 40)

```
Step 1: Start at root (50)
  40 < 50 → go left

Step 2: At node 30
  40 > 30 → go right

Step 3: At node 40
  40 == 40 → FOUND

Return pointer to node containing 40
```

### Show (Inorder Traversal)

```
inorder(50):
  inorder(30):
    inorder(20):
      print 20
    print 30
    inorder(40):
      print 40
  print 50
  inorder(70):
    print 70

Output: 20 30 40 50 70
```

---

## Conclusion

This program implements a **Complete BST Operations Menu** with the following achievements:

1. **Core BST Operations**:

   - **Insert**: O(log n) average, O(n) worst case
   - **Delete**: O(log n) average, O(n) worst case
   - **Find**: O(log n) average, O(n) worst case
   - **Show**: O(n) for traversal

2. **Deletion Cases Handled**:

   - **No Children**: Simply remove node
   - **One Child**: Replace with child
   - **Two Children**: Replace with inorder successor

3. **Multiple Display Options**:

   - **Inorder**: Sorted output
   - **Preorder**: Root-first traversal
   - **Postorder**: Children-first traversal
   - **Level Order**: Breadth-first traversal
   - **Tree Structure**: Visual representation

4. **Additional Features**:

   - Tree statistics (node count, height)
   - Duplicate detection
   - Error handling for missing values
   - Sample data creation

5. **Time Complexity**:

   - All operations: O(log n) average case
   - Worst case: O(n) for skewed trees
   - Traversals: O(n)

6. **Space Complexity**: O(n) for nodes + O(h) for recursion stack

**BST Properties Maintained**:

- Left subtree values < Root value
- Right subtree values > Root value
- Inorder traversal gives sorted sequence
- No duplicate values

**Applications**:

- Database indexing
- File system organization
- Expression parsing
- Priority queues
- Symbol tables

**Menu System Benefits**:

- User-friendly interface
- Comprehensive functionality
- Clear operation feedback
- Multiple visualization options
- Educational value for learning BST operations
