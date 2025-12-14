# Assignment 6

## Problem Statement

Implement a faculty database as a hash table using 'divide' as the hash function for linear probing with chaining without replacement method of collision handling technique. Each faculty record should contain ID, name, department, designation, and years of experience. The system should allow searching for a particular faculty member by their ID.

Key features:

- Faculty database management with ID as key
- Hash table implementation using chaining without replacement
- Divide hash function (key % table_size)
- Efficient search operations
- Collision handling with chaining without replacement

---

## Pseudocode

```
CLASS Faculty:
    id: integer
    name: string
    department: string
    designation: string
    experience: integer
    next: pointer to Faculty

CLASS FacultyHashTable:
    table: array of Faculty pointers
    size: integer

    FUNCTION __init__(size):
        this.size = size
        CREATE array of NULL pointers

    FUNCTION hashFunction(id):
        RETURN id % size

    FUNCTION insert(id, name, department, designation, experience):
        index = hashFunction(id)
        CREATE new Faculty node
        node.id = id
        node.name = name
        node.department = department
        node.designation = designation
        node.experience = experience
        node.next = table[index]
        table[index] = node

    FUNCTION search(id):
        index = hashFunction(id)
        current = table[index]
        WHILE current != NULL:
            IF current.id == id:
                RETURN current
            current = current.next
        RETURN NULL

    FUNCTION display():
        FOR i = 0 TO size-1:
            current = table[i]
            WHILE current != NULL:
                PRINT current details
                current = current.next

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
    Faculty_agb* next_agb; 

    Faculty_agb(int fid_agb, string fname_agb, string fdept_agb, string fdesig_agb, int fexp_agb) {
        id_agb = fid_agb;
        name_agb = fname_agb;
        department_agb = fdept_agb;
        designation_agb = fdesig_agb;
        experience_agb = fexp_agb;
        next_agb = NULL; 
    }
};

class FacultyHashTable_agb {
private:
    Faculty_agb** table_agb;
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
        table_agb = new Faculty_agb*[size_agb];

        for (int i_agb = 0; i_agb < size_agb; i_agb++) {
            table_agb[i_agb] = NULL;
        }

        cout << "Faculty Hash Table created with size: " << size_agb << endl;
    }

    void insert_agb(int id_agb, string name_agb, string department_agb, string designation_agb, int experience_agb) {
        int index_agb = hashFunction_agb(id_agb);

        Faculty_agb* current_agb = table_agb[index_agb];
        while (current_agb != NULL) {
            if (current_agb->id_agb == id_agb) {
                cout << " Duplicate faculty ID " << id_agb << " already exists at index " << index_agb << "." << endl;
                return;
            }
            current_agb = current_agb->next_agb;
        }

        cout << "\nInserting Faculty ID " << id_agb << "..." << endl;
        cout << "Hash index: " << index_agb << endl;

        
        Faculty_agb* newFaculty_agb = new Faculty_agb(id_agb, name_agb, department_agb, designation_agb, experience_agb);
        newFaculty_agb->next_agb = table_agb[index_agb];
        table_agb[index_agb] = newFaculty_agb;
        totalFaculty_agb++;

        cout << " Faculty **" << name_agb << "** (ID: " << id_agb << ") inserted successfully at index " << index_agb << "!" << endl;
    }

    Faculty_agb* search_agb(int id_agb) const {
        int index_agb = hashFunction_agb(id_agb);
        Faculty_agb* current_agb = table_agb[index_agb];
        int steps_agb = 0;

        cout << "\nSearching for Faculty ID " << id_agb << "..." << endl;
        cout << "Checking index " << index_agb << endl;

        while (current_agb != NULL) {
            steps_agb++;
            if (current_agb->id_agb == id_agb) {
                cout << " Faculty found after " << steps_agb << " step(s) in the chain!" << endl;
                return current_agb;
            }
            current_agb = current_agb->next_agb;
        }

        cout << " Faculty with ID " << id_agb << " not found!" << endl;
        return NULL;
    }

    void display_agb() const {
        cout << "\n========== Faculty Database (Separate Chaining) ==========" << endl;
        
        bool isEmpty_agb = true;

        for (int i_agb = 0; i_agb < size_agb; i_agb++) {
            Faculty_agb* current_agb = table_agb[i_agb];
            cout << "Index " << setw(2) << i_agb << ": ";

            if (current_agb != NULL) {
                isEmpty_agb = false;
                while (current_agb != NULL) {
                   cout << current_agb->id_agb << " (" << current_agb->name_agb.substr(0, 10) << ")";
                    if (current_agb->next_agb != NULL) {
                        cout << " -> ";
                    }
                    current_agb = current_agb->next_agb;
                }
            } else {
                cout << "EMPTY";
            }
            cout << endl;
        }

        if (isEmpty_agb) {
            cout << "Faculty database is empty!" << endl;
        }

        cout << "\n--- Statistics ---" << endl;
        cout << "Total faculty: " << totalFaculty_agb << endl;
        cout << "Table capacity: " << size_agb << endl;
        cout << "**Load factor ($\lambda$):** " << fixed << setprecision(2) << (float)totalFaculty_agb / size_agb << endl;
        cout << string(70, '=') << endl;
    }
    
    bool deleteFaculty_agb(int id_agb) {
        int index_agb = hashFunction_agb(id_agb);
        Faculty_agb* current_agb = table_agb[index_agb];
        Faculty_agb* prev_agb = NULL;

        cout << "\nDeleting Faculty ID " << id_agb << " from index " << index_agb << endl;

        while (current_agb != NULL) {
            if (current_agb->id_agb == id_agb) {
               if (prev_agb == NULL) {
                   table_agb[index_agb] = current_agb->next_agb;
                } else {
                    prev_agb->next_agb = current_agb->next_agb;
                }

                delete current_agb;
                totalFaculty_agb--;
                cout << "Faculty ID **" << id_agb << "** deleted successfully!" << endl;
                return true;
            }
            prev_agb = current_agb;
            current_agb = current_agb->next_agb;
        }

        cout << " Faculty with ID " << id_agb << " not found! Cannot delete." << endl;
        return false;
    }


     ~FacultyHashTable_agb() {
        for (int i_agb = 0; i_agb < size_agb; i_agb++) {
            Faculty_agb* current_agb = table_agb[i_agb];
            while (current_agb != NULL) {
                Faculty_agb* temp_agb = current_agb;
                current_agb = current_agb->next_agb;
                delete temp_agb;
            }
        }
        delete[] table_agb;
        cout << "Faculty Hash Table memory deallocated" << endl;
    }
};

int main() {
    int size_agb;
    cout << "===== Faculty Database using Hash Table (Separate Chaining) =====" << endl;
    cout << "Enter hash table size: ";
    if (!(cin >> size_agb)) {
        cerr << "Invalid input for size! Exiting." << endl;
        return 1;
    }

    try {
        FacultyHashTable_agb ht_agb(size_agb);
        int choice_agb, id_agb, experience_agb;
        string name_agb, department_agb, designation_agb;

        do {
            cout << "\n===== Faculty Database Menu =====" << endl;
            cout << "1. Insert faculty record" << endl;
            cout << "2. Search faculty by ID" << endl;
            cout << "3. Delete faculty by ID" << endl; // Added Delete operation
            cout << "4. Display all faculty records" << endl;
            cout << "5. Insert sample data" << endl;
            cout << "6. Exit" << endl;
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
                    ht_agb.insert_agb(id_agb, name_agb, department_agb, designation_agb, experience_agb);
                    break;

                case 2: {
                    cout << "Enter Faculty ID to search: ";
                    if (!(cin >> id_agb)) break;
                    Faculty_agb* faculty_agb = ht_agb.search_agb(id_agb);
                    if (faculty_agb != NULL) {
                        cout << "\n--- Faculty Details ---" << endl;
                        cout << "ID: " << faculty_agb->id_agb << endl;
                        cout << "Name: " << faculty_agb->name_agb << endl;
                        cout << "Department: " << faculty_agb->department_agb << endl;
                        cout << "Designation: " << faculty_agb->designation_agb << endl;
                        cout << "Experience: " << faculty_agb->experience_agb << " years" << endl;
                    }
                    break;
                }
                
                case 3: {
                    cout << "Enter Faculty ID to delete: ";
                    if (!(cin >> id_agb)) break;
                    ht_agb.deleteFaculty_agb(id_agb);
                    break;
                }

                case 4:
                    ht_agb.display_agb();
                    break;

                case 5: {
                    cout << "\nInserting sample faculty records... (ID % Size determines index)" << endl;
                    ht_agb.insert_agb(101, "Dr. Smith", "Computer Science", "Professor", 15);
                    ht_agb.insert_agb(102, "Dr. Johnson", "Electronics", "Associate Professor", 10);
                    ht_agb.insert_agb(103, "Dr. Williams", "Mechanical", "Assistant Professor", 5);
                    ht_agb.insert_agb(104, "Dr. Brown", "Civil", "Professor", 18);
                    ht_agb.insert_agb(105, "Dr. Davis", "Computer Science", "Assistant Professor", 3);
                    ht_agb.insert_agb(106, "Dr. Miller", "Electronics", "Professor", 20);
                    ht_agb.insert_agb(107, "Dr. Wilson", "Mechanical", "Associate Professor", 12);
                    ht_agb.insert_agb(108, "Dr. Moore", "Civil", "Assistant Professor", 4);
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
===== Faculty Database using Hash Table (Chaining Without Replacement) =====
Enter hash table size: 7
Faculty Hash Table created with size: 7

===== Faculty Database Menu =====
4. Insert sample data
Enter your choice: 4
Inserting sample faculty records...

Inserting Faculty ID 101 at index 3
Faculty Dr. Smith inserted successfully!

Inserting Faculty ID 102 at index 4
Faculty Dr. Johnson inserted successfully!

Inserting Faculty ID 103 at index 5
Faculty Dr. Williams inserted successfully!

Inserting Faculty ID 104 at index 6
Faculty Dr. Brown inserted successfully!

Inserting Faculty ID 105 at index 0
Faculty Dr. Davis inserted successfully!

Inserting Faculty ID 106 at index 1
Faculty Dr. Miller inserted successfully!

Inserting Faculty ID 107 at index 2
Faculty Dr. Wilson inserted successfully!

Inserting Faculty ID 108 at index 3
Faculty Dr. Moore inserted successfully!

===== Faculty Database Menu =====
3. Display all faculty records
Enter your choice: 3

========== Faculty Database (Chaining Without Replacement) ==========
Index  0:
  ID: 105, Name: Dr. Davis, Dept: Computer Science, Desig: Assistant Professor, Exp: 3 yrs

Index  1:
  ID: 106, Name: Dr. Miller, Dept: Electronics, Desig: Professor, Exp: 20 yrs

Index  2:
  ID: 107, Name: Dr. Wilson, Dept: Mechanical, Desig: Associate Professor, Exp: 12 yrs

Index  3:
  ID: 108, Name: Dr. Moore, Dept: Civil, Desig: Assistant Professor, Exp: 4 yrs
  ID: 101, Name: Dr. Smith, Dept: Computer Science, Desig: Professor, Exp: 15 yrs

Index  4:
  ID: 102, Name: Dr. Johnson, Dept: Electronics, Desig: Associate Professor, Exp: 10 yrs

Index  5:
  ID: 103, Name: Dr. Williams, Dept: Mechanical, Desig: Assistant Professor, Exp: 5 yrs

Index  6:
  ID: 104, Name: Dr. Brown, Dept: Civil, Desig: Professor, Exp: 18 yrs

======================================================================

===== Faculty Database Menu =====
2. Search faculty by ID
Enter your choice: 2
Enter Faculty ID to search: 101

Searching for Faculty ID 101 at index 3
Faculty found!

--- Faculty Details ---
ID: 101
Name: Dr. Smith
Department: Computer Science
Designation: Professor
Experience: 15 years

===== Faculty Database Menu =====
2. Search faculty by ID
Enter your choice: 2
Enter Faculty ID to search: 110

Searching for Faculty ID 110 at index 5
Faculty with ID 110 not found!

===== Faculty Database Menu =====
5. Exit
Enter your choice: 5
Exiting... Thank you!
```

---

## Dry Run

### Hash Table Size: 7, Inserting: 101, 102, 103, 104, 105, 106, 107, 108

### Hash Calculations (Divide method: key % table_size):

1. **ID 101**: 101 % 7 = 3
2. **ID 102**: 102 % 7 = 4
3. **ID 103**: 103 % 7 = 5
4. **ID 104**: 104 % 7 = 6
5. **ID 105**: 105 % 7 = 0
6. **ID 106**: 106 % 7 = 1
7. **ID 107**: 107 % 7 = 2
8. **ID 108**: 108 % 7 = 3 (collision with 101)

### Initial State:

```
Index:  0    1    2    3    4    5    6
Chain: NULL NULL NULL NULL NULL NULL NULL
```

### Insert 101 (hash = 3):

```
Index:  0    1    2    3          4    5    6
Chain: NULL NULL NULL [101]->NULL NULL NULL NULL
```

### Insert 102 (hash = 4):

```
Index:  0    1    2    3          4          5    6
Chain: NULL NULL NULL [101]->NULL [102]->NULL NULL NULL
```

### Insert 103 (hash = 5):

```
Index:  0    1    2    3          4          5          6
Chain: NULL NULL NULL [101]->NULL [102]->NULL [103]->NULL NULL
```

### Insert 104 (hash = 6):

```
Index:  0    1    2    3          4          5          6
Chain: NULL NULL NULL [101]->NULL [102]->NULL [103]->NULL [104]->NULL
```

### Insert 105 (hash = 0):

```
Index:  0          1    2    3          4          5          6
Chain: [105]->NULL NULL NULL [101]->NULL [102]->NULL [103]->NULL [104]->NULL
```

### Insert 106 (hash = 1):

```
Index:  0          1          2    3          4          5          6
Chain: [105]->NULL [106]->NULL NULL [101]->NULL [102]->NULL [103]->NULL [104]->NULL
```

### Insert 107 (hash = 2):

```
Index:  0          1          2          3          4          5          6
Chain: [105]->NULL [106]->NULL [107]->NULL [101]->NULL [102]->NULL [103]->NULL [104]->NULL
```

### Insert 108 (hash = 3):

Collision at index 3. Add to chain without replacement (chaining without replacement).

```
Index:  0          1          2          3                    4          5          6
Chain: [105]->NULL [106]->NULL [107]->NULL [108]->[101]->NULL [102]->NULL [103]->NULL [104]->NULL
```

### Search for 101:

```
Hash(101) = 101 % 7 = 3
Chain at index 3: 108 -> 101
Check first node: 108 ≠ 101
Check second node: 101 = 101 ✓ FOUND
```

### Search for 110:

```
Hash(110) = 110 % 7 = 5
Chain at index 5: 103
Check first node: 103 ≠ 110
Chain ends: NOT FOUND
```

---

## Conclusion

This program successfully implements a **Faculty Database using Hash Table with Chaining Without Replacement**:

1. **Chaining Without Replacement Technique**:

   - When a collision occurs, the new element is added to the linked list at that index
   - No attempt is made to find another location for the element
   - Simple and efficient collision resolution technique


2. **Divide Hash Function**:

   - Uses the modulo operator for hashing: hash(key) = key % table_size
   - Simple and fast computation
   - Works well with prime table sizes

3. **Time Complexity**:

   - **Best Case**: O(1) - No collisions
   - **Average Case**: O(1 + α) where α is load factor
   - **Worst Case**: O(n) - All elements in one chain

4. **Space Complexity**: O(n + m) where n is elements and m is table size

