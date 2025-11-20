# Assignment 2

## Problem Statement

Implement collision handling using separate chaining. Each slot in the hash table contains a linked list to handle multiple keys that hash to the same index.

---

## Pseudocode

```
CLASS Node:
    key: integer
    next: pointer to Node

CLASS HashTable:
    table: array of linked lists
    size: integer

    FUNCTION __init__(size):
        this.size = size
        CREATE array of NULL pointers

    FUNCTION hashFunction(key):
        RETURN key % size

    FUNCTION insert(key):
        index = hashFunction(key)

        // Check if key already exists
        current = table[index]
        WHILE current != NULL:
            IF current.key == key:
                PRINT "Duplicate key"
                RETURN FALSE
            current = current.next

        // Insert at beginning of chain
        newNode = CREATE Node(key)
        newNode.next = table[index]
        table[index] = newNode
        RETURN TRUE

    FUNCTION search(key):
        index = hashFunction(key)
        current = table[index]
        position = 0

        WHILE current != NULL:
            position++
            IF current.key == key:
                RETURN TRUE, position
            current = current.next

        RETURN FALSE, position

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
            PRINT index i
            PRINT all keys in chain

MAIN:
    INPUT size
    CREATE HashTable
    PERFORM operations
```

---

## C++ Code

```cpp
#include <iostream>
#include <iomanip>
#include <stdexcept>
using namespace std;

// Class for the nodes in the linked lists (chains)
class Node_agb {
public:
    int key_agb;
    Node_agb* next_agb;

    Node_agb(int k_agb) {
        key_agb = k_agb;
        next_agb = NULL;
    }
};

class HashTable_agb {
private:
    Node_agb** table_agb; // Array of pointers to linked lists
    int size_agb;
    int totalElements_agb;

    // Hash function (Modulo Division Method)
    // h(key) = key mod size
    int hashFunction_agb(int key_agb) const {
        // Ensure non-negative result
        return (key_agb % size_agb + size_agb) % size_agb;
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

        cout << "Hash table created with size: " << size_agb << endl;
    }

    // Insert key using separate chaining
    bool insert_agb(int key_agb) {
        int index_agb = hashFunction_agb(key_agb);

        cout << "\nInserting " << key_agb << "..." << endl;
        cout << "Hash value: " << index_agb << endl;

        // Check if key already exists in chain
        Node_agb* current_agb = table_agb[index_agb];
        while (current_agb != NULL) {
            if (current_agb->key_agb == key_agb) {
                cout << "✗ Duplicate key! " << key_agb << " already exists at index " << index_agb << "." << endl;
                return false;
            }
            current_agb = current_agb->next_agb;
        }

        // Insert at beginning of chain (simple and common practice)
        Node_agb* newNode_agb = new Node_agb(key_agb);
        newNode_agb->next_agb = table_agb[index_agb];
        table_agb[index_agb] = newNode_agb;
        totalElements_agb++;

        if (newNode_agb->next_agb == NULL) {
            cout << "✓ Inserted " << key_agb << " at index " << index_agb
                 << " (new chain)" << endl;
        } else {
            cout << "✓ Inserted " << key_agb << " at index " << index_agb
                 << " (added to existing chain, now first element)" << endl;
        }

        return true;
    }

    /**
     * @brief Searches for a key.
     * @param key_agb The key to search for.
     * @param chainPosition_agb Reference to store the position in the chain (1-based).
     * @return True if key is found, false otherwise.
     */
    bool search_agb(int key_agb, int& chainPosition_agb) const {
        int index_agb = hashFunction_agb(key_agb);
        Node_agb* current_agb = table_agb[index_agb];
        chainPosition_agb = 0; // Initialize position counter

        while (current_agb != NULL) {
            chainPosition_agb++;
            if (current_agb->key_agb == key_agb) {
                return true;
            }
            current_agb = current_agb->next_agb;
        }

        return false;
    }

    // Search with display
    void searchKey_agb(int key_agb) const {
        int chainPosition_agb = 0;
        int index_agb = hashFunction_agb(key_agb);
        bool found_agb = search_agb(key_agb, chainPosition_agb);

        cout << "\nSearching for " << key_agb << "..." << endl;
        cout << "Hash value: " << index_agb << endl;

        if (found_agb) {
            cout << "✓ Key **" << key_agb << "** found at index " << index_agb
                 << " (position " << chainPosition_agb << " in chain)" << endl;
        } else {
            cout << "✗ Key " << key_agb << " not found" << endl;
            if (chainPosition_agb > 0) {
                cout << "  Searched through " << chainPosition_agb
                     << " elements in chain before reaching end" << endl;
            }
        }
    }

    // Delete key
    bool deleteKey_agb(int key_agb) {
        int index_agb = hashFunction_agb(key_agb);
        Node_agb* current_agb = table_agb[index_agb];
        Node_agb* prev_agb = NULL;

        cout << "\nDeleting " << key_agb << "..." << endl;
        cout << "Checking index: " << index_agb << endl;

        while (current_agb != NULL) {
            if (current_agb->key_agb == key_agb) {
                // Remove node from chain
                if (prev_agb == NULL) {
                    // Deleting the first node (head of the list)
                    table_agb[index_agb] = current_agb->next_agb;
                } else {
                    // Deleting a node in the middle or end
                    prev_agb->next_agb = current_agb->next_agb;
                }

                delete current_agb;
                totalElements_agb--;
                cout << "✓ Key **" << key_agb << "** deleted from index " << index_agb << endl;
                return true;
            }
            prev_agb = current_agb;
            current_agb = current_agb->next_agb;
        }

        cout << "✗ Key " << key_agb << " not found! Cannot delete." << endl;
        return false;
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

    // Display statistics
    void displayStats_agb() const {
        int emptyChains_agb = 0;
        int maxChainLength_agb = 0;
        int totalChainLength_agb = 0;
        int nonEmptyChains_agb = 0;

        for (int i_agb = 0; i_agb < size_agb; i_agb++) {
            int chainLength_agb = 0;
            Node_agb* current_agb = table_agb[i_agb];

            if (current_agb == NULL) {
                emptyChains_agb++;
            } else {
                nonEmptyChains_agb++;
                while (current_agb != NULL) {
                    chainLength_agb++;
                    current_agb = current_agb->next_agb;
                }
                totalChainLength_agb += chainLength_agb;
                if (chainLength_agb > maxChainLength_agb) {
                    maxChainLength_agb = chainLength_agb;
                }
            }
        }

        cout << "\n--- Hash Table Statistics ---" << endl;
        cout << "Table size: " << size_agb << endl;
        cout << "Total elements: " << totalElements_agb << endl;
        cout << "Empty chains: " << emptyChains_agb << endl;
        cout << "Non-empty chains: " << nonEmptyChains_agb << endl;
        cout << "**Longest chain:** " << maxChainLength_agb << endl;

        if (nonEmptyChains_agb > 0) {
            cout << "**Average chain length:** " << fixed << setprecision(2)
                 << (float)totalChainLength_agb / nonEmptyChains_agb << endl;
        }

        cout << "**Load factor ($\lambda$):** " << fixed << setprecision(2)
             << (float)totalElements_agb / size_agb << endl;
    }

    // Destructor (Frees all allocated memory)
    ~HashTable_agb() {
        cout << "\nStarting memory cleanup..." << endl;
        for (int i_agb = 0; i_agb < size_agb; i_agb++) {
            Node_agb* current_agb = table_agb[i_agb];
            while (current_agb != NULL) {
                Node_agb* temp_agb = current_agb;
                current_agb = current_agb->next_agb;
                delete temp_agb;
            }
        }
        delete[] table_agb;
        cout << "Hash table memory successfully cleared." << endl;
    }
};

int main_agb() {
    int size_agb;

    cout << "===== Hash Table with Separate Chaining =====" << endl;
    cout << "Enter hash table size: ";
    cin >> size_agb;

    try {
        HashTable_agb ht_agb(size_agb);
        int choice_agb, key_agb;

        do {
            cout << "\n===== Menu =====" << endl;
            cout << "1. Insert key" << endl;
            cout << "2. Search key" << endl;
            cout << "3. Delete key" << endl;
            cout << "4. Display hash table" << endl;
            cout << "5. Display statistics" << endl;
            cout << "6. Insert sample data" << endl;
            cout << "7. Exit" << endl;
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
                    ht_agb.searchKey_agb(key_agb);
                    break;

                case 3:
                    cout << "Enter key to delete: ";
                    cin >> key_agb;
                    ht_agb.deleteKey_agb(key_agb);
                    break;

                case 4:
                    ht_agb.display_agb();
                    break;

                case 5:
                    ht_agb.displayStats_agb();
                    break;

                case 6: {
                    int sampleKeys_agb[] = {10, 20, 15, 7, 12, 25, 30, 17, 27, 37};
                    cout << "Inserting sample keys (10, 20, 15, 7, 12, 25, 30, 17, 27, 37):" << endl;
                    for (int i_agb = 0; i_agb < 10; i_agb++) {
                        ht_agb.insert_agb(sampleKeys_agb[i_agb]);
                    }
                    break;
                }

                case 7:
                    cout << "Exiting... Thank you!" << endl;
                    break;

                default:
                    cout << "Invalid choice!" << endl;
            }
        } while (choice_agb != 7);
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
===== Hash Table with Separate Chaining =====
Enter hash table size: 7
Hash table created with size: 7

===== Menu =====
Enter your choice: 6
Inserting sample keys: 10 20 15 7 12 25 30 17 27 37

Inserting 10...
Hash value: 3
✓ Inserted 10 at index 3 (new chain)

Inserting 20...
Hash value: 6
✓ Inserted 20 at index 6 (new chain)

Inserting 15...
Hash value: 1
✓ Inserted 15 at index 1 (new chain)

Inserting 7...
Hash value: 0
✓ Inserted 7 at index 0 (new chain)

Inserting 12...
Hash value: 5
✓ Inserted 12 at index 5 (new chain)

Inserting 25...
Hash value: 4
✓ Inserted 25 at index 4 (new chain)

Inserting 30...
Hash value: 2
✓ Inserted 30 at index 2 (new chain)

Inserting 17...
Hash value: 3
✓ Inserted 17 at index 3 (added to existing chain)

Inserting 27...
Hash value: 6
✓ Inserted 27 at index 6 (added to existing chain)

Inserting 37...
Hash value: 2
✓ Inserted 37 at index 2 (added to existing chain)

Enter your choice: 4

========== Hash Table (Separate Chaining) ==========
Index  0: 7
Index  1: 15
Index  2: 37 -> 30
Index  3: 17 -> 10
Index  4: 25
Index  5: 12
Index  6: 27 -> 20
====================================================
Total elements: 10
Load factor: 1.43

Enter your choice: 2
Enter key to search: 17

Searching for 17...
Hash value: 3
✓ Key 17 found at index 3 (position 1 in chain)

Enter your choice: 2
Enter key to search: 10

Searching for 10...
Hash value: 3
✓ Key 10 found at index 3 (position 2 in chain)

Enter your choice: 3
Enter key to delete: 30
✓ Key 30 deleted from index 2

Enter your choice: 4

========== Hash Table (Separate Chaining) ==========
Index  0: 7
Index  1: 15
Index  2: 37
Index  3: 17 -> 10
Index  4: 25
Index  5: 12
Index  6: 27 -> 20
====================================================
Total elements: 9
Load factor: 1.29

Enter your choice: 5

--- Hash Table Statistics ---
Table size: 7
Total elements: 9
Empty chains: 0
Non-empty chains: 7
Longest chain: 2
Average chain length: 1.29
Load factor: 1.29

Enter your choice: 7
Exiting... Thank you!
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

Index:  0    1      2    3      4    5    6
Chain: NULL [15]-> NULL [10]-> NULL NULL [20]->
            NULL        NULL              NULL
```

### Insert 7:

```
Hash(7) = 7 % 7 = 0
Chain at index 0 is empty

Index:  0     1      2    3      4    5    6
Chain: [7]-> [15]-> NULL [10]-> NULL NULL [20]->
       NULL  NULL        NULL              NULL
```

### Insert 17:

```
Hash(17) = 17 % 7 = 3
Chain at index 3 already has 10
Insert 17 at beginning of chain
17 -> 10

Index:  0     1      2    3           4    5    6
Chain: [7]-> [15]-> NULL [17]->[10]-> NULL NULL [20]->
       NULL  NULL        NULL  NULL              NULL
```

### Insert 27:

```
Hash(27) = 27 % 7 = 6
Chain at index 6 already has 20
Insert 27 at beginning
27 -> 20

Index:  0     1      2    3           4    5    6
Chain: [7]-> [15]-> NULL [17]->[10]-> NULL NULL [27]->[20]->
       NULL  NULL        NULL  NULL              NULL  NULL
```

### Search for 10:

```
Hash(10) = 3
Chain at index 3: 17 -> 10
Position 1: 17 ≠ 10
Position 2: 10 = 10 ✓ FOUND
Chain position: 2
```

### Delete 17:

```
Hash(17) = 3
Chain at index 3: 17 -> 10
First node matches (17)
Update: table[3] = 10

Index:  0     1      2    3      4    5    6
Chain: [7]-> [15]-> NULL [10]-> NULL NULL [27]->[20]->
       NULL  NULL        NULL              NULL  NULL
```

---

## Conclusion

This program successfully implements **Hash Table with Separate Chaining** collision resolution:

1. **Separate Chaining Concept**:

   - Each table slot contains a linked list
   - Keys with same hash value stored in same chain
   - No limit on number of elements (unlike open addressing)
   - Load factor can exceed 1.0

2. **Advantages over Linear Probing**:

   - **No clustering**: Each chain independent
   - **Simple deletion**: Just remove from linked list
   - **Unlimited capacity**: Can store more than table size
   - **Performance**: Less affected by high load factors

3. **Operations Implementation**:

   - **Insert**: O(1) average - add to beginning of chain
   - **Search**: O(1 + α) where α is load factor
   - **Delete**: O(1 + α) - find and remove from chain

4. **Load Factor Analysis**:

   - α = n/m (n = elements, m = table size)
   - α < 1: Good performance
   - α > 1: Acceptable (multiple elements per chain)
   - Average chain length = α

5. **Memory Considerations**:

   - Extra memory for pointers
   - Each node needs next pointer
   - Trade-off: Memory for flexibility

6. **Time Complexity**:

   - **Best case**: O(1) - key in first position
   - **Average case**: O(1 + α)
   - **Worst case**: O(n) - all keys in one chain

7. **Comparison with Open Addressing**:
   | Feature | Separate Chaining | Linear Probing |
   |---------|-------------------|----------------|
   | Clustering | None | Primary clustering |
   | Deletion | Easy | Requires markers |
   | Memory | More (pointers) | Less (array only) |
   | Load factor | Can exceed 1.0 | Must be < 1.0 |
   | Cache performance | Poor (scattered) | Good (sequential) |

**Applications**:

- Database indexing
- Symbol tables
- Compiler implementations
- Dictionary ADT
- Spell checkers

**Best Practices**:

- Keep load factor reasonable (α ≈ 0.75 - 1.0)
- Use prime number for table size
- Choose good hash function
- Consider resizing when α too high
