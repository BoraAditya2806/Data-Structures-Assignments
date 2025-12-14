# Assignment 5

## Problem Statement

Implement a faculty database as a hash table using MOD as the hash function with linear probing method for collision handling. The system should allow searching for a particular faculty member by their ID. Each faculty record should contain ID, name, department, designation, and years of experience.

Key features:

- Faculty database management with ID as key
- Hash table implementation using linear probing
- MOD hash function (key % table_size)
- Efficient search operations
- Collision handling with linear probing

---

## Pseudocode

```
CLASS Faculty:
    id: integer
    name: string
    department: string
    designation: string
    experience: integer
    isEmpty: boolean

CLASS FacultyHashTable:
    table: array of Faculty
    size: integer
    totalFaculty: integer

    FUNCTION __init__(size):
        this.size = size
        totalFaculty = 0
        CREATE array of Faculty objects
        FOR each element in array:
            element.isEmpty = TRUE

    FUNCTION hashFunction(id):
        RETURN id % size

    FUNCTION insert(id, name, department, designation, experience):
        index = hashFunction(id)
        originalIndex = index

        WHILE table[index].isEmpty == FALSE AND table[index].id != id:
            index = (index + 1) % size
            IF index == originalIndex:
                PRINT "Hash table is full"
                RETURN FALSE

        IF table[index].id == id:
            PRINT "Duplicate faculty ID"
            RETURN FALSE

        table[index].id = id
        table[index].name = name
        table[index].department = department
        table[index].designation = designation
        table[index].experience = experience
        table[index].isEmpty = FALSE
        totalFaculty++
        RETURN TRUE

    FUNCTION search(id):
        index = hashFunction(id)
        originalIndex = index

        WHILE table[index].isEmpty == FALSE:
            IF table[index].id == id:
                RETURN table[index]
            index = (index + 1) % size
            IF index == originalIndex:
                BREAK

        RETURN NULL

    FUNCTION display():
        FOR i = 0 TO size-1:
            IF table[i].isEmpty == FALSE:
                PRINT table[i] details

MAIN:
    CREATE FacultyHashTable
    INSERT sample faculty records
    PERFORM search operations
```

---

## C++ Code

```cpp
#include <iostream>
#include <iomanip>
#include <string>
#include <stdexcept>
using namespace std;

class Faculty_agb {
public:
    int id_agb;
    string name_agb;
    string department_agb;
    string designation_agb;
    int experience_agb;
    bool isEmpty_agb;

Faculty_agb() {
        isEmpty_agb = true; 
    }
};

class FacultyHashTable_agb {
private:
    Faculty_agb* table_agb;
    int size_agb;
    int totalFaculty_agb;

   int hashFunction_agb(int id_agb) const {
        return id_agb % size_agb;
    }

public:
    FacultyHashTable_agb(int s_agb) {
        if (s_agb <= 0) {
          throw invalid_argument("Hash table size must be positive.");
        }
        size_agb = s_agb;
        totalFaculty_agb = 0;
        table_agb = new Faculty_agb[size_agb];

        for (int i_agb = 0; i_agb < size_agb; i_agb++) {
            table_agb[i_agb].isEmpty_agb = true;
        }

        cout << "Faculty Hash Table created with capacity: " << size_agb << endl;
    }

    bool insert_agb(int id_agb, string name_agb, string department_agb, string designation_agb, int experience_agb) {
        if (totalFaculty_agb >= size_agb) {
            cout << " Hash table is FULL! Cannot insert more faculty." << endl;
            return false;
        }

        int originalIndex_agb = hashFunction_agb(id_agb);
        int index_agb = originalIndex_agb;
        int probeCount_agb = 0;

        cout << "\nInserting Faculty ID " << id_agb << "..." << endl;
        cout << "Initial Hash index: " << originalIndex_agb << endl;

        // Linear probing to find an empty slot
        do {
            // 1. Check for duplicate ID in the slot
            if (!table_agb[index_agb].isEmpty_agb && table_agb[index_agb].id_agb == id_agb) {
                cout << " Duplicate faculty ID! Faculty with ID " << id_agb << " already exists at index " << index_agb << "." << endl;
                return false;
            }

           
            if (table_agb[index_agb].isEmpty_agb) {
                break; 
            }

            
            probeCount_agb++;
            index_agb = (index_agb + 1) % size_agb;

          
            if (index_agb == originalIndex_agb) {
                cout << " Hash table is logically FULL! Cannot insert more faculty." << endl;
                return false;
            }

        } while (true);

       
        table_agb[index_agb].id_agb = id_agb;
        table_agb[index_agb].name_agb = name_agb;
        table_agb[index_agb].department_agb = department_agb;
        table_agb[index_agb].designation_agb = designation_agb;
        table_agb[index_agb].experience_agb = experience_agb;
        table_agb[index_agb].isEmpty_agb = false; // Mark slot as occupied
        totalFaculty_agb++;

        cout << " Faculty **" << name_agb << "** (ID: " << id_agb << ") inserted at index " << index_agb;
        if (probeCount_agb > 0) {
            cout << " (after " << probeCount_agb << " probes)";
        }
        cout << " successfully!" << endl;
        return true;
    }
    Faculty_agb* search_agb(int id_agb) const {
        int originalIndex_agb = hashFunction_agb(id_agb);
        int index_agb = originalIndex_agb;
        int probeCount_agb = 0;

        cout << "\nSearching for Faculty ID " << id_agb << "..." << endl;
        cout << "Initial Hash index: " << originalIndex_agb << endl;

        
        do {
            if (table_agb[index_agb].isEmpty_agb) {
                break;
            }

            if (table_agb[index_agb].id_agb == id_agb) {
                cout << " Faculty found at index " << index_agb;
                if (probeCount_agb > 0) {
                    cout << " (after " << probeCount_agb << " probes)";
                }
                cout << "!" << endl;
                return &table_agb[index_agb];
            }

            // Move to the next slot
            probeCount_agb++;
            index_agb = (index_agb + 1) % size_agb;

            // Check if we've traversed the entire table
            if (index_agb == originalIndex_agb) {
                break;
            }
        } while (true);

        cout << " Faculty with ID " << id_agb << " not found!" << endl;
        return nullptr;
    }

    // Display all faculty records
    void display_agb() const {
        cout << "\n========== Faculty Database (Linear Probing) ==========" << endl;
        
        cout << setw(5) << "Index" << setw(10) << "ID" << setw(18) << "Name" << setw(14) << "Department" << setw(18) << "Designation" << setw(6) << "Exp" << endl;
        cout << string(71, '-') << endl;

        bool isEmpty_agb = true;

        for (int i_agb = 0; i_agb < size_agb; i_agb++) {
            cout << setw(5) << i_agb;
            if (!table_agb[i_agb].isEmpty_agb) {
                isEmpty_agb = false;
                cout << setw(10) << table_agb[i_agb].id_agb
                     << setw(18) << table_agb[i_agb].name_agb
                     << setw(14) << table_agb[i_agb].department_agb
                     << setw(18) << table_agb[i_agb].designation_agb
                     << setw(6) << table_agb[i_agb].experience_agb << " yrs" << endl;
            } else {
                // Displaying empty slots for clarity in the table structure
                cout << setw(10) << "---" << setw(18) << "EMPTY" << setw(14) << "---" << setw(18) << "---" << setw(6) << "---" << endl;
            }
        }

        if (isEmpty_agb && totalFaculty_agb == 0) {
            cout << "\nNo faculty records found!" << endl;
        }

        cout << string(71, '-') << endl;
        cout << "\n--- Statistics ---" << endl;
        cout << "Total faculty: " << totalFaculty_agb << endl;
        cout << "Table capacity: " << size_agb << endl;
        cout << "**Load factor ($\lambda$):** " << fixed << setprecision(2) << (float)totalFaculty_agb / size_agb << endl;
        cout << string(34, '=') << endl;
    }
    ~FacultyHashTable_agb() {
        delete[] table_agb;
        cout << "Faculty Hash Table memory deallocated" << endl;
    }
};

int main() {
    int size_agb;

    cout << "===== Faculty Database using Hash Table (Linear Probing) =====" << endl;
    cout << "Enter hash table size: ";
    if (!(cin >> size_agb)) {
        cerr << "Invalid input for size! Exiting." << endl;
        return 1;
    }

    try {
        FacultyHashTable_agb facultyDB_agb(size_agb);
        int choice_agb, id_agb, experience_agb;
        string name_agb, department_agb, designation_agb;

        do {
            cout << "\n===== Faculty Database Menu =====" << endl;
            cout << "1. Insert faculty record" << endl;
            cout << "2. Search faculty by ID" << endl;
            cout << "3. Display all faculty records" << endl;
            cout << "4. Insert sample data" << endl;
            cout << "5. Exit" << endl;
            cout << "Enter your choice: ";
            if (!(cin >> choice_agb)) {
                cerr << "Invalid input! Exiting." << endl;
                break;
            }

            switch (choice_agb) {
                case 1:
                    cout << "Enter Faculty ID: ";
                    if (!(cin >> id_agb)) break;
                    cout << "Enter Name: ";
                    cin.ignore();
                    getline(cin, name_agb);
                    cout << "Enter Department: ";
                    getline(cin, department_agb);
                    cout << "Enter Designation: ";
                    getline(cin, designation_agb);
                    cout << "Enter Years of Experience: ";
                    if (!(cin >> experience_agb)) break;
                    facultyDB_agb.insert_agb(id_agb, name_agb, department_agb, designation_agb, experience_agb);
                    break;

                case 2: {
                    cout << "Enter Faculty ID to search: ";
                    if (!(cin >> id_agb)) break;
                    Faculty_agb* faculty_agb = facultyDB_agb.search_agb(id_agb);
                    if (faculty_agb != nullptr) {
                        cout << "\n--- Faculty Details ---" << endl;
                        cout << "ID: " << faculty_agb->id_agb << endl;
                        cout << "Name: " << faculty_agb->name_agb << endl;
                        cout << "Department: " << faculty_agb->department_agb << endl;
                        cout << "Designation: " << faculty_agb->designation_agb << endl;
                        cout << "Experience: " << faculty_agb->experience_agb << " years" << endl;
                    }
                    break;
                }

                case 3:
                    facultyDB_agb.display_agb();
                    break;

                case 4: {
                    cout << "\nInserting sample faculty records... (ID % Size determines index)" << endl;
                    facultyDB_agb.insert_agb(101, "Dr. Smith", "Computer Science", "Professor", 15);
                    facultyDB_agb.insert_agb(102, "Dr. Johnson", "Electronics", "Associate Professor", 10);
                    facultyDB_agb.insert_agb(103, "Dr. Williams", "Mechanical", "Assistant Professor", 5);
                    facultyDB_agb.insert_agb(104, "Dr. Brown", "Civil", "Professor", 18);
                    facultyDB_agb.insert_agb(105, "Dr. Davis", "Computer Science", "Assistant Professor", 3);
                    facultyDB_agb.insert_agb(106, "Dr. Miller", "Electronics", "Professor", 20);
                    facultyDB_agb.insert_agb(107, "Dr. Wilson", "Mechanical", "Associate Professor", 12);
                    facultyDB_agb.insert_agb(108, "Dr. Moore", "Civil", "Assistant Professor", 4);
                    break;
                }

                case 5:
                    cout << "Exiting... Thank you!" << endl;
                    break;

                default:
                    cout << "Invalid choice! Please try again." << endl;
            }
        } while (choice_agb != 5);

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
===== Faculty Database using Hash Table (Linear Probing) =====
Enter hash table size: 11
Faculty Hash Table created with capacity: 11

===== Faculty Database Menu =====
4. Insert sample data
Enter your choice: 4
Inserting sample faculty records...

Inserting Faculty ID 101...
Hash value: 2
Faculty Dr. Smith (ID: 101) inserted successfully!

Inserting Faculty ID 102...
Hash value: 3
Faculty Dr. Johnson (ID: 102) inserted successfully!

Inserting Faculty ID 103...
Hash value: 4
Faculty Dr. Williams (ID: 103) inserted successfully!

Inserting Faculty ID 104...
Hash value: 5
Faculty Dr. Brown (ID: 104) inserted successfully!

Inserting Faculty ID 105...
Hash value: 6
Faculty Dr. Davis (ID: 105) inserted successfully!

Inserting Faculty ID 106...
Hash value: 7
Faculty Dr. Miller (ID: 106) inserted successfully!

Inserting Faculty ID 107...
Hash value: 8
Faculty Dr. Wilson (ID: 107) inserted successfully!

Inserting Faculty ID 108...
Hash value: 9
Faculty Dr. Moore (ID: 108) inserted successfully!

===== Faculty Database Menu =====
3. Display all faculty records
Enter your choice: 3

========== Faculty Database ==========
ID:  101, Name:       Dr. Smith, Dept: Computer Science, Desig:        Professor, Exp: 15 yrs
ID:  102, Name:     Dr. Johnson, Dept:     Electronics, Desig: Associate Professor, Exp: 10 yrs
ID:  103, Name:   Dr. Williams, Dept:      Mechanical, Desig: Assistant Professor, Exp: 5 yrs
ID:  104, Name:       Dr. Brown, Dept:           Civil, Desig:        Professor, Exp: 18 yrs
ID:  105, Name:       Dr. Davis, Dept: Computer Science, Desig: Assistant Professor, Exp: 3 yrs
ID:  106, Name:      Dr. Miller, Dept:     Electronics, Desig:        Professor, Exp: 20 yrs
ID:  107, Name:      Dr. Wilson, Dept:      Mechanical, Desig: Associate Professor, Exp: 12 yrs
ID:  108, Name:       Dr. Moore, Dept:           Civil, Desig: Assistant Professor, Exp: 4 yrs

--- Statistics ---
Total faculty: 8
Table capacity: 11
Load factor: 0.73
==================================

===== Faculty Database Menu =====
2. Search faculty by ID
Enter your choice: 2
Enter Faculty ID to search: 106

Searching for Faculty ID 106...
Hash value: 7
Faculty found!

--- Faculty Details ---
ID: 106
Name: Dr. Miller
Department: Electronics
Designation: Professor
Experience: 20 years

===== Faculty Database Menu =====
2. Search faculty by ID
Enter your choice: 2
Enter Faculty ID to search: 112

Searching for Faculty ID 112...
Hash value: 2
Faculty with ID 112 not found!

===== Faculty Database Menu =====
5. Exit
Enter your choice: 5
Exiting... Thank you!
Faculty Hash Table memory deallocated
```

---

## Dry Run

### Hash Table Size: 11, Inserting: 101, 102, 103, 104, 105, 106, 107, 108

### Initial State:

```
Index:  0   1   2   3   4   5   6   7   8   9   10
Entry: [-] [-] [-] [-] [-] [-] [-] [-] [-] [-] [-]
       Empty slots
```

### Insert 101 (hash = 101 % 11 = 2):

```
Index:  0   1   2          3   4   5   6   7   8   9   10
Entry: [-] [-] [101:Smith] [-] [-] [-] [-] [-] [-] [-] [-]
```

### Insert 102 (hash = 102 % 11 = 3):

```
Index:  0   1   2          3             4   5   6   7   8   9   10
Entry: [-] [-] [101:Smith] [102:Johnson] [-] [-] [-] [-] [-] [-] [-]
```

### Insert 103 (hash = 103 % 11 = 4):

```
Index:  0   1   2          3             4                5   6   7   8   9   10
Entry: [-] [-] [101:Smith] [102:Johnson] [103:Williams] [-] [-] [-] [-] [-] [-]
```

### Insert 104 (hash = 104 % 11 = 5):

```
Index:  0   1   2          3             4                5           6   7   8   9   10
Entry: [-] [-] [101:Smith] [102:Johnson] [103:Williams] [104:Brown] [-] [-] [-] [-] [-]
```

### Insert 105 (hash = 105 % 11 = 6):

```
Index:  0   1   2          3             4                5           6          7   8   9   10
Entry: [-] [-] [101:Smith] [102:Johnson] [103:Williams] [104:Brown] [105:Davis] [-] [-] [-] [-]
```

### Insert 106 (hash = 106 % 11 = 7):

```
Index:  0   1   2          3             4                5           6          7            8   9   10
Entry: [-] [-] [101:Smith] [102:Johnson] [103:Williams] [104:Brown] [105:Davis] [106:Miller] [-] [-] [-]
```

### Insert 107 (hash = 107 % 11 = 8):

```
Index:  0   1   2          3             4                5           6          7            8           9   10
Entry: [-] [-] [101:Smith] [102:Johnson] [103:Williams] [104:Brown] [105:Davis] [106:Miller] [107:Wilson] [-] [-]
```

### Insert 108 (hash = 108 % 11 = 9):

```
Index:  0   1   2          3             4                5           6          7            8           9          10
Entry: [-] [-] [101:Smith] [102:Johnson] [103:Williams] [104:Brown] [105:Davis] [106:Miller] [107:Wilson] [108:Moore] [-]
```

### Search for 106:

```
Hash(106) = 106 % 11 = 7
Check index 7: ID 106 = 106 ✓ FOUND
```

### Search for 112:

```
Hash(112) = 112 % 11 = 2
Check index 2: ID 101 ≠ 112, continue probing
Check index 3: ID 102 ≠ 112, continue probing
Check index 4: ID 103 ≠ 112, continue probing
Check index 5: ID 104 ≠ 112, continue probing
Check index 6: ID 105 ≠ 112, continue probing
Check index 7: ID 106 ≠ 112, continue probing
Check index 8: ID 107 ≠ 112, continue probing
Check index 9: ID 108 ≠ 112, continue probing
Check index 10: EMPTY slot, STOP searching
Result: NOT FOUND
```

---

## Conclusion

This program successfully implements a **Faculty Database using Hash Table with Linear Probing**:

1. **Hash Table with Linear Probing**:

   - Uses MOD hash function (key % table_size)
   - Resolves collisions using linear probing technique
   - Efficient storage and retrieval of faculty records

2. **Key Features**:

   - Faculty records with ID, name, department, designation, and experience
   - Insert and search operations
   - Load factor calculation for performance monitoring
   - Duplicate prevention

3. **Linear Probing Technique**:

   - When a collision occurs, checks the next slot sequentially
   - Wraps around to the beginning if reaching the end
   - Simple and cache-friendly collision resolution

4. **MOD Hash Function**:

   - Uses the modulo operator for hashing: hash(key) = key % table_size
   - Simple and fast computation
   - Works well with prime table sizes

5. **Time Complexity**:

   - **Best Case**: O(1) - No collisions
   - **Average Case**: O(1) with low load factor
   - **Worst Case**: O(n) - All elements form a cluster

6. **Space Complexity**: O(n) where n is the table size
