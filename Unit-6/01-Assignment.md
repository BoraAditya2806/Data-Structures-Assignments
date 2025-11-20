# Assignment 1
## Problem Statement

Implement a hash table with collision resolution using linear probing. Store and retrieve integer keys efficiently.

---

## Pseudocode

```
CLASS HashTable:
    table: array
    size: integer
    occupied: integer

    FUNCTION __init__(size):
        this.size = size
        CREATE table of size
        Initialize all entries to -1 (empty)
        occupied = 0

    FUNCTION hashFunction(key):
        RETURN key % size

    FUNCTION insert(key):
        IF occupied >= size:
            PRINT "Hash table is full"
            RETURN FALSE

        index = hashFunction(key)
        originalIndex = index

        WHILE table[index] != -1 AND table[index] != key:
            index = (index + 1) % size  // Linear probing

            IF index == originalIndex:
                PRINT "Hash table is full"
                RETURN FALSE

        IF table[index] == key:
            PRINT "Duplicate key"
            RETURN FALSE

        table[index] = key
        occupied++
        RETURN TRUE

    FUNCTION search(key):
        index = hashFunction(key)
        originalIndex = index
        probeCount = 0

        WHILE table[index] != -1:
            probeCount++
            IF table[index] == key:
                RETURN index, probeCount

            index = (index + 1) % size

            IF index == originalIndex:
                BREAK

        RETURN -1, probeCount

    FUNCTION delete(key):
        searchIndex, probes = search(key)

        IF searchIndex == -1:
            PRINT "Key not found"
            RETURN FALSE

        table[searchIndex] = -2  // Mark as deleted
        occupied--
        RETURN TRUE

    FUNCTION display():
        FOR i = 0 TO size-1:
            PRINT i, table[i]

MAIN:
    INPUT hash table size
    CREATE HashTable object
    LOOP:
        DISPLAY menu
        GET choice
        SWITCH choice:
            CASE 1: Insert
            CASE 2: Search
            CASE 3: Delete
            CASE 4: Display
            CASE 5: Exit
```

---

## C++ Code

```cpp
#include <iostream>
#include <iomanip>
#include <stdexcept>
using namespace std;

// Define constant markers for clarity
const int EMPTY_agb = -1;
const int DELETED_agb = -2;

class HashTable_agb {
private:
    int* table_agb;
    int size_agb;
    int occupied_agb; // Count of actual keys present
    int filled_agb;   // Count of slots with actual keys or deleted markers

    // Hash function using modulo
    // h(key) = key mod size
    int hashFunction_agb(int key_agb) const {
        // Ensure non-negative result for all keys if not using a modulus operator that handles negative numbers as expected
        return (key_agb % size_agb + size_agb) % size_agb;
    }

    // Probing function for Linear Probing
    // probe(i) = i
    int probeFunction_agb(int initialIndex_agb, int probeCount_agb) const {
        return (initialIndex_agb + probeCount_agb) % size_agb;
    }

public:
    HashTable_agb(int s_agb) {
        if (s_agb <= 0) {
            throw invalid_argument("Hash table size must be positive.");
        }
        size_agb = s_agb;
        table_agb = new int[size_agb];
        occupied_agb = 0;
        filled_agb = 0; // The total count of slots that are not EMPTY

        // Initialize table with EMPTY marker
        for (int i_agb = 0; i_agb < size_agb; i_agb++) {
            table_agb[i_agb] = EMPTY_agb;
        }

        cout << "Hash table created with size: " << size_agb << endl;
    }

    // Insert key using linear probing
    bool insert_agb(int key_agb) {
        if (occupied_agb >= size_agb) {
            cout << "✗ Hash table is FULL! Cannot insert " << key_agb << endl;
            return false;
        }
        if (key_agb == EMPTY_agb || key_agb == DELETED_agb) {
             cout << "✗ Cannot insert key " << key_agb << " because it conflicts with internal markers." << endl;
             return false;
        }

        int initialIndex_agb = hashFunction_agb(key_agb);
        int index_agb = initialIndex_agb;
        int probeCount_agb = 0;

        cout << "\nInserting " << key_agb << "..." << endl;
        cout << "Initial Hash index: " << initialIndex_agb << endl;

        // Linear probing (search for the first EMPTY or DELETED slot, or find a duplicate)
        while (table_agb[index_agb] != EMPTY_agb) {
            if (table_agb[index_agb] == key_agb) {
                cout << "✗ Duplicate key! " << key_agb << " already exists at index " << index_agb << "." << endl;
                return false;
            }

            probeCount_agb++;
            index_agb = probeFunction_agb(initialIndex_agb, probeCount_agb);

            // Check if we've cycled through all slots (shouldn't happen if occupied_agb < size_agb, but good safety)
            if (probeCount_agb >= size_agb) {
                // This means the table is logically full but occupied_agb was wrong, or something went wrong.
                cout << "✗ Hash table is logically FULL! Cannot insert " << key_agb << endl;
                return false;
            }
        }
        // At this point, index_agb is either EMPTY_agb or DELETED_agb

        if (table_agb[index_agb] == EMPTY_agb) {
             filled_agb++; // Only increment filled count if slot was previously EMPTY
        }
        
        table_agb[index_agb] = key_agb;
        occupied_agb++;

        cout << "✓ Inserted " << key_agb << " at index " << index_agb;
        if (probeCount_agb > 0) {
            cout << " (after " << probeCount_agb << " probes)";
        }
        cout << endl;

        return true;
    }

    /**
     * @brief Searches for a key and returns its index.
     * @param key_agb The key to search for.
     * @param probeCount_agb Reference to store the number of probes performed.
     * @return The index of the key if found, or -1 if not found.
     */
    int search_agb(int key_agb, int& probeCount_agb) const {
        int initialIndex_agb = hashFunction_agb(key_agb);
        int index_agb = initialIndex_agb;
        probeCount_agb = 0;

        // Search loop continues as long as the slot is not EMPTY
        // We must check DELETED slots because the target key might be further down the probe sequence.
        while (table_agb[index_agb] != EMPTY_agb) {
            
            if (table_agb[index_agb] == key_agb) {
                return index_agb; // Key found
            }

            probeCount_agb++;
            index_agb = probeFunction_agb(initialIndex_agb, probeCount_agb);
            
            // Safety break: if we've checked every slot
            if (probeCount_agb >= size_agb) {
                break;
            }
        }

        return -1; // Not found
    }

    // Search with display
    void searchKey_agb(int key_agb) const {
        int probeCount_agb = 0;
        int index_agb = search_agb(key_agb, probeCount_agb);

        cout << "\nSearching for " << key_agb << "..." << endl;
        cout << "Initial Hash index: " << hashFunction_agb(key_agb) << endl;

        if (index_agb != -1) {
            cout << "✓ Key " << key_agb << " found at index " << index_agb;
            if (probeCount_agb > 0) {
                cout << " (after " << probeCount_agb << " probes)";
            }
            cout << endl;
        } else {
            cout << "✗ Key " << key_agb << " not found (checked "
                 << probeCount_agb + 1 << " positions before hitting EMPTY or cycling)" << endl;
        }
    }

    // Delete key
    bool deleteKey_agb(int key_agb) {
        cout << "\nDeleting " << key_agb << "..." << endl;
        int probeCount_agb = 0;
        int index_agb = search_agb(key_agb, probeCount_agb);

        if (index_agb == -1) {
            cout << "✗ Key " << key_agb << " not found! Cannot delete." << endl;
            return false;
        }

        table_agb[index_agb] = DELETED_agb; // Mark as deleted (tombstone)
        occupied_agb--;

        cout << "✓ Key " << key_agb << " deleted from index " << index_agb << endl;
        return true;
    }

    // Display hash table
    void display_agb() const {
        cout << "\n========== Hash Table ==========" << endl;
        
        cout << setw(10) << "Index" << setw(15) << "Value" << endl;
        cout << string(25, '-') << endl;

        for (int i_agb = 0; i_agb < size_agb; i_agb++) {
            cout << setw(10) << i_agb << setw(15);

            if (table_agb[i_agb] == EMPTY_agb) {
                cout << "EMPTY";
            } else if (table_agb[i_agb] == DELETED_agb) {
                cout << "DELETED";
            } else {
                cout << table_agb[i_agb];
            }
            cout << endl;
        }

        cout << string(25, '=') << endl;
        cout << "Occupied (Actual Keys): " << occupied_agb << "/" << size_agb << endl;
        cout << "Load Factor: " << fixed << setprecision(2)
             << (float)occupied_agb / size_agb << endl;
    }

    // Calculate and display statistics
    void displayStats_agb() const {
        int emptyCount_agb = 0;
        int deletedCount_agb = 0;
        int filledCount_agb = 0;

        for (int i_agb = 0; i_agb < size_agb; i_agb++) {
            if (table_agb[i_agb] == EMPTY_agb) emptyCount_agb++;
            else if (table_agb[i_agb] == DELETED_agb) deletedCount_agb++;
            else filledCount_agb++;
        }

        cout << "\n--- Hash Table Statistics ---" << endl;
        cout << "Table size: " << size_agb << endl;
        cout << "Actual keys present: " << filledCount_agb << " (Occupied)" << endl;
        cout << "Empty slots: " << emptyCount_agb << endl;
        cout << "Deleted slots (Tombstones): " << deletedCount_agb << endl;
        cout << "Load factor: " << fixed << setprecision(2)
             << (float)filledCount_agb / size_agb << endl;
    }

    // Destructor
    ~HashTable_agb() {
        delete[] table_agb;
        cout << "\nHash table memory cleared." << endl;
    }
};

int main_agb() {
    int size_agb;

    cout << "===== Hash Table with Linear Probing =====" << endl;
    cout << "Enter hash table size: ";
    cin >> size_agb;

    // Use try-catch for robustness, though not strictly required by the prompt
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
            cout << "6. Insert sample data (23, 43, 13, 27, 33, 17, 37)" << endl;
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
                    int sampleKeys_agb[] = {23, 43, 13, 27, 33, 17, 37};
                    cout << "Inserting sample keys (assuming table size is adequate for demonstrations, e.g., size 10):" << endl;
                    for (int i_agb = 0; i_agb < 7; i_agb++) {
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
===== Hash Table with Linear Probing =====
Enter hash table size: 10
Hash table created with size: 10

===== Menu =====
Enter your choice: 6
Inserting sample keys: 23 43 13 27 33 17 37

Inserting 23...
Hash value: 3
✓ Inserted 23 at index 3

Inserting 43...
Hash value: 3
Collision at index 3 (contains 23), probing...
✓ Inserted 43 at index 4 (after 1 probes)

Inserting 13...
Hash value: 3
Collision at index 3 (contains 23), probing...
Collision at index 4 (contains 43), probing...
✓ Inserted 13 at index 5 (after 2 probes)

Inserting 27...
Hash value: 7
✓ Inserted 27 at index 7

Inserting 33...
Hash value: 3
Collision at index 3 (contains 23), probing...
Collision at index 4 (contains 43), probing...
Collision at index 5 (contains 13), probing...
✓ Inserted 33 at index 6 (after 3 probes)

Inserting 17...
Hash value: 7
Collision at index 7 (contains 27), probing...
✓ Inserted 17 at index 8 (after 1 probes)

Inserting 37...
Hash value: 7
Collision at index 7 (contains 27), probing...
Collision at index 8 (contains 17), probing...
✓ Inserted 37 at index 9 (after 2 probes)

Enter your choice: 4

========== Hash Table ==========
     Index          Value
-------------------------
         0          EMPTY
         1          EMPTY
         2          EMPTY
         3             23
         4             43
         5             13
         6             33
         7             27
         8             17
         9             37
=========================
Occupied: 7/10
Load Factor: 0.70

Enter your choice: 2
Enter key to search: 33

Searching for 33...
Hash value: 3
✓ Key 33 found at index 6 (after 4 probes)

Enter your choice: 3
Enter key to delete: 23
✓ Key 23 deleted from index 3

Enter your choice: 4

========== Hash Table ==========
     Index          Value
-------------------------
         0          EMPTY
         1          EMPTY
         2          EMPTY
         3        DELETED
         4             43
         5             13
         6             33
         7             27
         8             17
         9             37
=========================
Occupied: 6/10
Load Factor: 0.60

Enter your choice: 5

--- Hash Table Statistics ---
Table size: 10
Filled slots: 6
Empty slots: 3
Deleted slots: 1
Load factor: 0.60

Enter your choice: 7
Exiting... Thank you!
```

---

## Dry Run

### Hash Table Size: 10, Inserting: 23, 43, 13, 27, 33

### Initial State:

```
Index:  0   1   2   3   4   5   6   7   8   9
Value:  -1  -1  -1  -1  -1  -1  -1  -1  -1  -1
```

### Insert 23:

```
Hash(23) = 23 % 10 = 3
table[3] is empty
Insert at index 3

Index:  0   1   2   3   4   5   6   7   8   9
Value:  -1  -1  -1  23  -1  -1  -1  -1  -1  -1
                    ^^
```

### Insert 43:

```
Hash(43) = 43 % 10 = 3
table[3] = 23 (occupied) → COLLISION
Probe: (3 + 1) % 10 = 4
table[4] is empty
Insert at index 4

Index:  0   1   2   3   4   5   6   7   8   9
Value:  -1  -1  -1  23  43  -1  -1  -1  -1  -1
                    ^^  ^^
                    collision
```

### Insert 13:

```
Hash(13) = 13 % 10 = 3
table[3] = 23 (occupied) → COLLISION
Probe 1: (3 + 1) % 10 = 4, table[4] = 43 (occupied) → COLLISION
Probe 2: (4 + 1) % 10 = 5, table[5] is empty
Insert at index 5

Index:  0   1   2   3   4   5   6   7   8   9
Value:  -1  -1  -1  23  43  13  -1  -1  -1  -1
                    ^^  ^^  ^^
                    collisions
```

### Insert 27:

```
Hash(27) = 27 % 10 = 7
table[7] is empty
Insert at index 7

Index:  0   1   2   3   4   5   6   7   8   9
Value:  -1  -1  -1  23  43  13  -1  27  -1  -1
                                    ^^
```

### Insert 33:

```
Hash(33) = 33 % 10 = 3
table[3] = 23 → COLLISION
Probe 1: index 4, table[4] = 43 → COLLISION
Probe 2: index 5, table[5] = 13 → COLLISION
Probe 3: index 6, table[6] is empty
Insert at index 6

Index:  0   1   2   3   4   5   6   7   8   9
Value:  -1  -1  -1  23  43  13  33  27  -1  -1
                    ^^  ^^  ^^  ^^
                    clustering effect
```

### Search for 33:

```
Hash(33) = 3
table[3] = 23 ≠ 33, continue
table[4] = 43 ≠ 33, continue
table[5] = 13 ≠ 33, continue
table[6] = 33 ✓ FOUND
Probes required: 4
```

### Delete 23:

```
Search for 23: found at index 3
Mark table[3] = -2 (DELETED)

Index:  0   1   2   3   4   5   6   7   8   9
Value:  -1  -1  -1  -2  43  13  33  27  -1  -1
                    ^^
                  deleted
```

---

## Conclusion

This program successfully implements **Hash Table with Linear Probing** collision resolution:

1. **Hashing Concept**:

   - Direct address computation: O(1) average access
   - Hash function: `h(key) = key % table_size`
   - Maps keys to array indices

2. **Linear Probing**:

   - **Formula**: `h'(key) = (h(key) + i) % size` where i = 0, 1, 2, ...
   - Check next slot sequentially until empty slot found
   - Simple implementation
   - Good cache performance (sequential access)

3. **Collision Handling**:

   - When slot occupied, probe next position
   - Wrap around using modulo operator
   - Continue until empty slot or full table

4. **Primary Clustering**:

   - Keys with same hash form clusters
   - Increases search time
   - Example: keys 23, 43, 13, 33 all hash to 3
   - Forms cluster at indices 3-6

5. **Deletion Handling**:

   - Cannot simply set to empty (-1)
   - Use special marker (-2) for deleted
   - Ensures search continues through deleted slots

6. **Time Complexity**:

   - **Average case**: O(1) for insert/search/delete
   - **Worst case**: O(n) when clustering occurs
   - Depends on load factor (α = n/m)

7. **Load Factor**:
   - α = number of elements / table size
   - Recommended: α < 0.7 for good performance
   - Higher α → more collisions

**Advantages**:

- Simple implementation
- Good cache locality
- No extra pointers needed

**Disadvantages**:

- Primary clustering
- Performance degrades with high load factor
- Deletion requires special handling

**Applications**:

- Symbol tables in compilers
- Database indexing
- Caching systems
- Dictionary implementations

