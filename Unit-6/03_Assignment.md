# Assignment 3

## Problem Statement

Implement a hash table that uses linked lists for collision resolution. When multiple keys hash to the same index, they should be stored in a linked list at that index. The implementation should support insertion, searching, deletion, and display operations.

Key features:

- Hash table with array of linked lists
- Collision resolution using separate chaining
- Dynamic memory management
- Comprehensive error handling

---

## Pseudocode

```
CLASS Node:
    key: integer
    next: pointer to Node

CLASS HashTable:
    table: array of Node pointers
    size: integer

    FUNCTION __init__(size):
        this.size = size
        CREATE array of NULL pointers

    FUNCTION hashFunction(key):
        RETURN key % size

    FUNCTION insert(key):
        index = hashFunction(key)
        CREATE new Node with key
        newNode.next = table[index]
        table[index] = newNode

    FUNCTION search(key):
        index = hashFunction(key)
        current = table[index]
        WHILE current != NULL:
            IF current.key == key:
                RETURN TRUE
            current = current.next
        RETURN FALSE

    FUNCTION delete(key):
        index = hashFunction(key)
        current = table[index]
        prev = NULL

        WHILE current != NULL:
            IF current.key == key:
                IF prev == NULL:
                    table[index] = current.next
                ELSE:
                    prev.next = current.next
                DELETE current
                RETURN TRUE
            prev = current
            current = current.next
        RETURN FALSE

    FUNCTION display():
        FOR i = 0 TO size-1:
            PRINT "Index " + i + ": "
            current = table[i]
            WHILE current != NULL:
                PRINT current.key + " -> "
                current = current.next
            PRINT "NULL"

MAIN:
    CREATE HashTable
    PERFORM insert, search, delete, display operations
```

---

## C++ Code

```cpp
#include <iostream>
#include <iomanip>
#include <stdexcept> // Include for exceptions like invalid_argument
using namespace std;

// Node Class for the Linked List (Chain) 🔗
class Node_agb {
public:
    int key_agb;
    Node_agb* next_agb;

    Node_agb(int k_agb) {
        key_agb = k_agb;
        next_agb = NULL;
    }
};

// Hash Table Class implementing Separate Chaining
class HashTable_agb {
private:
    Node_agb** table_agb; // Array of pointers to linked lists (the chains)
    int size_agb;
    int totalElements_agb;

    // Hash function (Modulo Division Method)
    // h(key) = key mod size
    int hashFunction_agb(int key_agb) const {
        // Use hash function from the original code: key % size
        return key_agb % size_agb;
    }

public:
    HashTable_agb(int s_agb) {
        if (s_agb <= 0) {
            throw invalid_argument("Hash table size must be positive.");
        }
        size_agb = s_agb;
        totalElements_agb = 0;
        table_agb = new Node_agb*[size_agb];

        // Initialize all chains as empty (NULL)
        for (int i_agb = 0; i_agb < size_agb; i_agb++) {
            table_agb[i_agb] = NULL;
        }

        cout << "Hash Table created with size: " << size_agb << endl;
    }

    // Insert key into hash table using Separate Chaining (Insert at Head)
    void insert_agb(int key_agb) {
        int index_agb = hashFunction_agb(key_agb);

        // Check for duplicates before insertion
        Node_agb* checkCurrent_agb = table_agb[index_agb];
        while (checkCurrent_agb != NULL) {
            if (checkCurrent_agb->key_agb == key_agb) {
                cout << "✗ Duplicate key " << key_agb << " already exists at index " << index_agb << "." << endl;
                return;
            }
            checkCurrent_agb = checkCurrent_agb->next_agb;
        }

        cout << "\nInserting " << key_agb << "..." << endl;
        cout << "Hash index: " << index_agb << endl;

        // Insert at the beginning of the linked list (chain)
        Node_agb* newNode_agb = new Node_agb(key_agb);
        newNode_agb->next_agb = table_agb[index_agb];
        table_agb[index_agb] = newNode_agb;
        totalElements_agb++;

        cout << "✓ Key **" << key_agb << "** inserted successfully at index " << index_agb << "!" << endl;
    }

    // Search for a key
    bool search_agb(int key_agb) const {
        int index_agb = hashFunction_agb(key_agb);
        Node_agb* current_agb = table_agb[index_agb];
        int steps_agb = 0;

        cout << "\nSearching for " << key_agb << "..." << endl;
        cout << "Checking index " << index_agb << " (Chain length: " << getChainLength_agb(index_agb) << ")" << endl;

        while (current_agb != NULL) {
            steps_agb++;
            if (current_agb->key_agb == key_agb) {
                cout << "✓ Key **" << key_agb << "** found after " << steps_agb << " step(s) in the chain!" << endl;
                return true;
            }
            current_agb = current_agb->next_agb;
        }

        cout << "✗ Key " << key_agb << " not found in the chain (searched " << steps_agb << " steps)." << endl;
        return false;
    }

    // Delete a key
    bool deleteKey_agb(int key_agb) {
        int index_agb = hashFunction_agb(key_agb);
        Node_agb* current_agb = table_agb[index_agb];
        Node_agb* prev_agb = NULL;

        cout << "\nDeleting " << key_agb << "..." << endl;
        cout << "Checking index " << index_agb << endl;

        while (current_agb != NULL) {
            if (current_agb->key_agb == key_agb) {
                // Remove node from chain
                if (prev_agb == NULL) {
                    // Deleting the first node (head)
                    table_agb[index_agb] = current_agb->next_agb;
                } else {
                    // Deleting a node in the middle or end
                    prev_agb->next_agb = current_agb->next_agb;
                }

                delete current_agb;
                totalElements_agb--;
                cout << "✓ Key **" << key_agb << "** deleted successfully from index " << index_agb << "!" << endl;
                return true;
            }
            prev_agb = current_agb;
            current_agb = current_agb->next_agb;
        }

        cout << "✗ Key " << key_agb << " not found! Cannot delete." << endl;
        return false;
    }

    // Helper function to get chain length
    int getChainLength_agb(int index_agb) const {
        int length_agb = 0;
        Node_agb* current_agb = table_agb[index_agb];
        while (current_agb != NULL) {
            length_agb++;
            current_agb = current_agb->next_agb;
        }
        return length_agb;
    }

    // Display hash table
    void display_agb() const {
        cout << "\n========== Hash Table (Separate Chaining) ==========" << endl;
        

        for (int i_agb = 0; i_agb < size_agb; i_agb++) {
            cout << "Index " << setw(2) << i_agb << ": ";

            Node_agb* current_agb = table_agb[i_agb];
            if (current_agb == NULL) {
                cout << "EMPTY";
            } else {
                while (current_agb != NULL) {
                    cout << current_agb->key_agb;
                    if (current_agb->next_agb != NULL) {
                        cout << " -> ";
                    }
                    current_agb = current_agb->next_agb;
                }
            }
            cout << endl;
        }

        cout << string(52, '=') << endl;
        cout << "Total elements: " << totalElements_agb << endl;
        cout << "Load factor ($\lambda$): " << fixed << setprecision(2)
             << (float)totalElements_agb / size_agb << endl;
    }

    // Destructor to free memory
    ~HashTable_agb() {
        for (int i_agb = 0; i_agb < size_agb; i_agb++) {
            Node_agb* current_agb = table_agb[i_agb];
            while (current_agb != NULL) {
                Node_agb* temp_agb = current_agb;
                current_agb = current_agb->next_agb;
                delete temp_agb;
            }
        }
        delete[] table_agb;
        cout << "Hash Table memory deallocated" << endl;
    }
};

int main_agb() {
    int size_agb;

    cout << "===== Hash Table with Linked Lists (Separate Chaining) =====" << endl;
    cout << "Enter hash table size: ";
    cin >> size_agb;

    // Use try-catch for safe allocation
    try {
        HashTable_agb ht_agb(size_agb);
        int choice_agb, key_agb;

        do {
            cout << "\n===== Hash Table Menu =====" << endl;
            cout << "1. Insert key" << endl;
            cout << "2. Search key" << endl;
            cout << "3. Delete key" << endl;
            cout << "4. Display hash table" << endl;
            cout << "5. Insert sample data (10, 20, 15, 7, 12, 25, 30)" << endl;
            cout << "6. Exit" << endl;
            cout << "Enter your choice: ";
            cin >> choice_agb;

            switch (choice_agb) {
                case 1:
                    cout << "Enter key to insert: ";
                    cin >> key_agb;
                    ht_agb.insert_agb(key_agb);
                    break;

                case 2:
                    cout << "Enter key to search: ";
                    cin >> key_agb;
                    ht_agb.search_agb(key_agb);
                    break;

                case 3:
                    cout << "Enter key to delete: ";
                    cin >> key_agb;
                    ht_agb.deleteKey_agb(key_agb);
                    break;

                case 4:
                    ht_agb.display_agb();
                    break;

                case 5: {
                    cout << "Inserting sample keys: 10, 20, 15, 7, 12, 25, 30" << endl;
                    int sampleKeys_agb[] = {10, 20, 15, 7, 12, 25, 30};
                    for (int i_agb = 0; i_agb < 7; i_agb++) {
                        ht_agb.insert_agb(sampleKeys_agb[i_agb]);
                    }
                    break;
                }

                case 6:
                    cout << "Exiting... Thank you!" << endl;
                    break;

                default:
                    cout << "Invalid choice! Please try again." << endl;
            }
        } while (choice_agb != 6);
    } catch (const exception& e) {
        cerr << "An error occurred: " << e.what() << endl;
        return 1;
    }

    return 0;
}
```

---

## Input/Output

### Sample Run:

```
===== Hash Table with Linked Lists =====
Enter hash table size: 7
Hash Table created with size: 7

===== Hash Table Menu =====
1. Insert key
2. Search key
3. Delete key
4. Display hash table
5. Insert sample data
6. Exit
Enter your choice: 5
Inserting sample keys: 10, 20, 15, 7, 12, 25, 30
Inserting 10 at index 3
✓ Key 10 inserted successfully!
Inserting 20 at index 6
✓ Key 20 inserted successfully!
Inserting 15 at index 1
✓ Key 15 inserted successfully!
Inserting 7 at index 0
✓ Key 7 inserted successfully!
Inserting 12 at index 5
✓ Key 12 inserted successfully!
Inserting 25 at index 4
✓ Key 25 inserted successfully!
Inserting 30 at index 2
✓ Key 30 inserted successfully!

===== Hash Table Menu =====
4. Display hash table
Enter your choice: 4

========== Hash Table (Linked Lists) ==========
Index  0: 7
Index  1: 15
Index  2: 30
Index  3: 10
Index  4: 25
Index  5: 12
Index  6: 20
==============================================

===== Hash Table Menu =====
1. Insert key
Enter your choice: 1
Enter key to insert: 17
Inserting 17 at index 3
✓ Key 17 inserted successfully!

===== Hash Table Menu =====
1. Insert key
Enter your choice: 1
Enter key to insert: 27
Inserting 27 at index 6
✓ Key 27 inserted successfully!

===== Hash Table Menu =====
4. Display hash table
Enter your choice: 4

========== Hash Table (Linked Lists) ==========
Index  0: 7
Index  1: 15
Index  2: 30
Index  3: 17 -> 10
Index  4: 25
Index  5: 12
Index  6: 27 -> 20
==============================================

===== Hash Table Menu =====
2. Search key
Enter your choice: 2
Enter key to search: 17
Searching for 17 at index 3
✓ Key 17 found!

===== Hash Table Menu =====
3. Delete key
Enter your choice: 3
Enter key to delete: 20
Deleting 20 from index 6
✓ Key 20 deleted successfully!

===== Hash Table Menu =====
4. Display hash table
Enter your choice: 4

========== Hash Table (Linked Lists) ==========
Index  0: 7
Index  1: 15
Index  2: 30
Index  3: 17 -> 10
Index  4: 25
Index  5: 12
Index  6: 27
==============================================

===== Hash Table Menu =====
6. Exit
Enter your choice: 6
Exiting... Thank you!
Hash Table memory deallocated
```

---

## Dry Run

### Hash Table Size: 7, Inserting: 10, 20, 15, 7, 17, 27

### Initial State:

```
Index:  0    1    2    3    4    5    6
Chain: NULL NULL NULL NULL NULL NULL NULL
```

### Insert 10:

```
Hash(10) = 10 % 7 = 3
Chain at index 3 is empty
Create node with key 10

Index:  0    1    2    3      4    5    6
Chain: NULL NULL NULL [10]-> NULL NULL NULL
                      NULL
```

### Insert 20:

```
Hash(20) = 20 % 7 = 6
Chain at index 6 is empty
Create node with key 20

Index:  0    1    2    3      4    5    6
Chain: NULL NULL NULL [10]-> NULL NULL [20]->
                      NULL              NULL
```

### Insert 15:

```
Hash(15) = 15 % 7 = 1
Chain at index 1 is empty
Create node with key 15

Index:  0    1      2    3      4    5    6
Chain: NULL [15]-> NULL [10]-> NULL NULL [20]->
            NULL        NULL              NULL
```

### Insert 7:

```
Hash(7) = 7 % 7 = 0
Chain at index 0 is empty
Create node with key 7

Index:  0     1      2    3      4    5    6
Chain: [7]-> [15]-> NULL [10]-> NULL NULL [20]->
       NULL  NULL        NULL              NULL
```

### Insert 17:

```
Hash(17) = 17 % 7 = 3
Chain at index 3 already has 10
Insert 17 at beginning of chain (17 -> 10)

Index:  0     1      2    3           4    5    6
Chain: [7]-> [15]-> NULL [17]->[10]-> NULL NULL [20]->
       NULL  NULL        NULL  NULL              NULL
```

### Insert 27:

```
Hash(27) = 27 % 7 = 6
Chain at index 6 already has 20
Insert 27 at beginning of chain (27 -> 20)

Index:  0     1      2    3           4    5    6
Chain: [7]-> [15]-> NULL [17]->[10]-> NULL NULL [27]->[20]->
       NULL  NULL        NULL  NULL              NULL  NULL
```

### Search for 17:

```
Hash(17) = 17 % 7 = 3
Chain at index 3: 17 -> 10
Check first node: 17 = 17 ✓ FOUND
```

### Delete 20:

```
Hash(20) = 20 % 7 = 6
Chain at index 6: 27 -> 20
Traverse chain to find 20
Found at second position
Update links: 27 -> NULL
Delete node with key 20

Index:  0     1      2    3           4    5    6
Chain: [7]-> [15]-> NULL [17]->[10]-> NULL NULL [27]->
       NULL  NULL        NULL  NULL              NULL
```

---

## Conclusion

This program successfully implements a **Hash Table with Linked Lists for Collision Resolution**:

1. **Separate Chaining Technique**:

   - Each slot in the hash table contains a linked list
   - Multiple keys that hash to the same index are stored in the same chain
   - No limit on the number of elements that can be stored

2. **Key Operations**:

   - **Insert**: O(1) average case - insert at beginning of chain
   - **Search**: O(1 + α) where α is the load factor
   - **Delete**: O(1 + α) - find and remove from chain

3. **Advantages**:

   - **No Clustering**: Each chain is independent
   - **Simple Deletion**: Just remove from linked list
   - **Unlimited Capacity**: Can store more elements than table size
   - **Performance**: Less affected by high load factors

4. **Time Complexity**:

   - **Best Case**: O(1) - key in first position of chain
   - **Average Case**: O(1 + α) where α = n/m (load factor)
   - **Worst Case**: O(n) - all keys in one chain

5. **Space Complexity**: O(n + m) where n is number of elements and m is table size

6. **Applications**:
   - Database indexing
   - Symbol tables in compilers
   - Caching systems
   - Dictionary implementations
   - Spell checkers

**Best Practices**:

- Keep load factor reasonable (α ≈ 0.75 - 1.0)
- Use prime number for table size
- Choose good hash function for uniform distribution
- Consider resizing when load factor becomes too high
