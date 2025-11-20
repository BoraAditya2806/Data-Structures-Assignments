# Assignment 4

## Problem Statement

Implement a system to store and retrieve student records using roll numbers as keys. Use hashing techniques to allow efficient insertion, search, and deletion of student records. Each student record should contain roll number, name, branch, and CGPA.

Key features:

- Student record management with roll number as key
- Hash table implementation for efficient operations
- Collision handling using linear probing
- Menu-driven interface for user interaction

---

## Pseudocode

```
CLASS Student:
    rollNo: integer
    name: string
    branch: string
    cgpa: float
    isEmpty: boolean

CLASS StudentHashTable:
    table: array of Student
    size: integer
    totalStudents: integer

    FUNCTION __init__(size):
        this.size = size
        totalStudents = 0
        CREATE array of Student objects
        FOR each element in array:
            element.isEmpty = TRUE

    FUNCTION hashFunction(rollNo):
        RETURN rollNo % size

    FUNCTION insert(rollNo, name, branch, cgpa):
        index = hashFunction(rollNo)
        originalIndex = index

        WHILE table[index].isEmpty == FALSE AND table[index].rollNo != rollNo:
            index = (index + 1) % size
            IF index == originalIndex:
                PRINT "Hash table is full"
                RETURN FALSE

        IF table[index].rollNo == rollNo:
            PRINT "Duplicate roll number"
            RETURN FALSE

        table[index].rollNo = rollNo
        table[index].name = name
        table[index].branch = branch
        table[index].cgpa = cgpa
        table[index].isEmpty = FALSE
        totalStudents++
        RETURN TRUE

    FUNCTION search(rollNo):
        index = hashFunction(rollNo)
        originalIndex = index

        WHILE table[index].isEmpty == FALSE:
            IF table[index].rollNo == rollNo:
                RETURN table[index]
            index = (index + 1) % size
            IF index == originalIndex:
                BREAK

        RETURN NULL

    FUNCTION delete(rollNo):
        index = hashFunction(rollNo)
        originalIndex = index

        WHILE table[index].isEmpty == FALSE:
            IF table[index].rollNo == rollNo:
                table[index].isEmpty = TRUE
                totalStudents--
                RETURN TRUE
            index = (index + 1) % size
            IF index == originalIndex:
                BREAK

        RETURN FALSE

    FUNCTION display():
        FOR i = 0 TO size-1:
            IF table[i].isEmpty == FALSE:
                PRINT table[i] details

MAIN:
    CREATE StudentHashTable
    INSERT sample student records
    PERFORM search, delete, display operations
```

---

## C++ Code

```cpp
#include <iostream>
#include <iomanip>
#include <string>
#include <stdexcept> // For better error handling
using namespace std;

// Student Record Structure 🎓
class Student_agb {
public:
    int rollNo_agb;
    string name_agb;
    string branch_agb;
    float cgpa_agb;
    // We use a simple boolean marker for 'empty' status.
    // In a production-level hash table using linear probing, a separate 'DELETED' marker
    // is essential for correct searching after deletion, which is missing here.
    // However, following the user's logic:
    bool isEmpty_agb;

    Student_agb() {
        isEmpty_agb = true; // Default constructor marks slot as empty
    }
};

// Hash Table Class for Student Records using Linear Probing
class StudentHashTable_agb {
private:
    Student_agb* table_agb;
    int size_agb;
    int totalStudents_agb;

    // Hash function: Roll Number modulo Table Size
    int hashFunction_agb(int rollNo_agb) const {
        return rollNo_agb % size_agb;
    }

public:
    StudentHashTable_agb(int s_agb) {
        if (s_agb <= 0) {
            throw invalid_argument("Hash table size must be positive.");
        }
        size_agb = s_agb;
        totalStudents_agb = 0;
        table_agb = new Student_agb[size_agb];
        cout << "Student Hash Table created with capacity: " << size_agb << endl;
    }

    // Insert student record using Linear Probing
    bool insert_agb(int rollNo_agb, string name_agb, string branch_agb, float cgpa_agb) {
        if (totalStudents_agb >= size_agb) {
            cout << "✗ Hash table is FULL! Cannot insert more students." << endl;
            return false;
        }

        int originalIndex_agb = hashFunction_agb(rollNo_agb);
        int index_agb = originalIndex_agb;
        int probeCount_agb = 0;

        cout << "\nInserting Student Roll No " << rollNo_agb << "..." << endl;
        cout << "Initial Hash index: " << originalIndex_agb << endl;

        // Linear probing to find an empty slot (or a duplicate)
        do {
            if (table_agb[index_agb].rollNo_agb == rollNo_agb && !table_agb[index_agb].isEmpty_agb) {
                // Check for duplicate roll number
                cout << "✗ Duplicate roll number! Student with roll no " << rollNo_agb << " already exists at index " << index_agb << "." << endl;
                return false;
            }

            if (table_agb[index_agb].isEmpty_agb) {
                // Found an empty slot, proceed to insert
                break;
            }

            // Collision occurred, move to the next slot
            probeCount_agb++;
            index_agb = (index_agb + 1) % size_agb;

            // Check if we've traversed the entire table
            if (index_agb == originalIndex_agb) {
                // This condition should have been caught by the totalStudents check, but acts as a safeguard
                cout << "✗ Hash table is logically FULL! Cannot insert more students." << endl;
                return false;
            }

        } while (true);

        // Insert the student record
        table_agb[index_agb].rollNo_agb = rollNo_agb;
        table_agb[index_agb].name_agb = name_agb;
        table_agb[index_agb].branch_agb = branch_agb;
        table_agb[index_agb].cgpa_agb = cgpa_agb;
        table_agb[index_agb].isEmpty_agb = false; // Mark slot as occupied
        totalStudents_agb++;

        cout << "✓ Student **" << name_agb << "** (Roll No: " << rollNo_agb << ") inserted at index " << index_agb;
        if (probeCount_agb > 0) {
            cout << " (after " << probeCount_agb << " probes)";
        }
        cout << " successfully!" << endl;
        return true;
    }

    // Search for a student record
    Student_agb* search_agb(int rollNo_agb) const {
        int originalIndex_agb = hashFunction_agb(rollNo_agb);
        int index_agb = originalIndex_agb;
        int probeCount_agb = 0;

        cout << "\nSearching for Student Roll No " << rollNo_agb << "..." << endl;
        cout << "Initial Hash index: " << originalIndex_agb << endl;

        // Linear probing to find the student
        do {
            if (table_agb[index_agb].isEmpty_agb) {
                // Empty slot found: search terminates (assuming no proper DELETED tombstone is used)
                break;
            }

            if (table_agb[index_agb].rollNo_agb == rollNo_agb) {
                cout << "✓ Student found at index " << index_agb;
                if (probeCount_agb > 0) {
                    cout << " (after " << probeCount_agb << " probes)";
                }
                cout << "!" << endl;
                return &table_agb[index_agb];
            }

            // Move to the next slot
            probeCount_agb++;
            index_agb = (index_agb + 1) % size_agb;

            // Check if we've traversed the entire table (and looped back)
            if (index_agb == originalIndex_agb) {
                break;
            }
        } while (true);

        cout << "✗ Student with Roll No " << rollNo_agb << " not found!" << endl;
        return nullptr;
    }

    // Delete a student record
    bool deleteStudent_agb(int rollNo_agb) {
        // NOTE: The original code sets isEmpty = true, which is inadequate for linear probing
        // because it breaks the search path for subsequent elements that probed past this deleted slot.
        // A proper implementation requires a 'DELETED' tombstone marker.
        // Following the original logic, but highlighting the limitation.

        int originalIndex_agb = hashFunction_agb(rollNo_agb);
        int index_agb = originalIndex_agb;

        cout << "\nDeleting Student Roll No " << rollNo_agb << "..." << endl;
        cout << "Hash value: " << originalIndex_agb << endl;

        // Linear probing to find the student
        do {
            if (table_agb[index_agb].isEmpty_agb) {
                break; // Empty slot means key isn't present
            }

            if (table_agb[index_agb].rollNo_agb == rollNo_agb) {
                // Delete operation: Mark as empty (should be a 'DELETED' marker for correctness)
                table_agb[index_agb].isEmpty_agb = true;
                totalStudents_agb--;
                cout << "✓ Student with Roll No **" << rollNo_agb << "** deleted successfully from index " << index_agb << "!" << endl;
                return true;
            }

            // Move to the next slot
            index_agb = (index_agb + 1) % size_agb;

            // Check if we've traversed the entire table
            if (index_agb == originalIndex_agb) {
                break;
            }
        } while (true);

        cout << "✗ Student with Roll No " << rollNo_agb << " not found! Cannot delete." << endl;
        return false;
    }

    // Display all student records
    void display_agb() const {
        cout << "\n========== Student Records ==========" << endl;
        
        cout << setw(5) << "Index" << setw(10) << "Roll No" << setw(18) << "Name" << setw(12) << "Branch" << setw(8) << "CGPA" << endl;
        cout << string(53, '-') << endl;

        bool isEmpty_agb = true;

        for (int i_agb = 0; i_agb < size_agb; i_agb++) {
            cout << setw(5) << i_agb;
            if (!table_agb[i_agb].isEmpty_agb) {
                isEmpty_agb = false;
                cout << setw(10) << table_agb[i_agb].rollNo_agb
                     << setw(18) << table_agb[i_agb].name_agb
                     << setw(12) << table_agb[i_agb].branch_agb
                     << setw(8) << fixed << setprecision(2) << table_agb[i_agb].cgpa_agb << endl;
            } else {
                // Displaying empty slots for clarity in the table structure
                cout << setw(10) << "---" << setw(18) << "EMPTY" << setw(12) << "---" << setw(8) << "---" << endl;
            }
        }

        if (isEmpty_agb && totalStudents_agb == 0) {
            cout << "\nNo student records found!" << endl;
        }

        cout << string(53, '-') << endl;
        cout << "\n--- Statistics ---" << endl;
        cout << "Total students: " << totalStudents_agb << endl;
        cout << "Table capacity: " << size_agb << endl;
        cout << "Load factor ($\lambda$): " << fixed << setprecision(2) << (float)totalStudents_agb / size_agb << endl;
        cout << string(34, '=') << endl;
    }

    // Destructor
    ~StudentHashTable_agb() {
        delete[] table_agb;
        cout << "Student Hash Table memory deallocated" << endl;
    }
};

int main_agb() {
    int size_agb;

    cout << "===== Student Records Management System (Linear Probing) =====" << endl;
    cout << "Enter hash table size: ";
    if (!(cin >> size_agb)) {
        cerr << "Invalid input for size!" << endl;
        return 1;
    }

    try {
        StudentHashTable_agb studentDB_agb(size_agb);
        int choice_agb, rollNo_agb;
        string name_agb, branch_agb;
        float cgpa_agb;

        do {
            cout << "\n===== Student Database Menu =====" << endl;
            cout << "1. Insert student record" << endl;
            cout << "2. Search student by roll number" << endl;
            cout << "3. Delete student record" << endl;
            cout << "4. Display all student records" << endl;
            cout << "5. Insert sample data" << endl;
            cout << "6. Exit" << endl;
            cout << "Enter your choice: ";
            if (!(cin >> choice_agb)) {
                cerr << "Invalid input! Exiting." << endl;
                break;
            }

            switch (choice_agb) {
                case 1:
                    cout << "Enter Roll Number: ";
                    if (!(cin >> rollNo_agb)) break;
                    cout << "Enter Name: ";
                    cin.ignore();
                    getline(cin, name_agb);
                    cout << "Enter Branch: ";
                    getline(cin, branch_agb);
                    cout << "Enter CGPA: ";
                    if (!(cin >> cgpa_agb)) break;
                    studentDB_agb.insert_agb(rollNo_agb, name_agb, branch_agb, cgpa_agb);
                    break;

                case 2: {
                    cout << "Enter Roll Number to search: ";
                    if (!(cin >> rollNo_agb)) break;
                    Student_agb* student_agb = studentDB_agb.search_agb(rollNo_agb);
                    if (student_agb != nullptr) {
                        cout << "\n--- Student Details ---" << endl;
                        cout << "Roll No: " << student_agb->rollNo_agb << endl;
                        cout << "Name: " << student_agb->name_agb << endl;
                        cout << "Branch: " << student_agb->branch_agb << endl;
                        cout << "CGPA: " << fixed << setprecision(2) << student_agb->cgpa_agb << endl;
                    }
                    break;
                }

                case 3: {
                    cout << "Enter Roll Number to delete: ";
                    if (!(cin >> rollNo_agb)) break;
                    studentDB_agb.deleteStudent_agb(rollNo_agb);
                    break;
                }

                case 4:
                    studentDB_agb.display_agb();
                    break;

                case 5: {
                    cout << "\nInserting sample student records... (RNo % Size determines index)" << endl;
                    // Ensure size is large enough for these 8 entries without immediately filling
                    studentDB_agb.insert_agb(101, "Alice Johnson", "Computer Science", 8.75); // RNo 101
                    studentDB_agb.insert_agb(102, "Bob Smith", "Electronics", 7.90);     // RNo 102
                    studentDB_agb.insert_agb(103, "Charlie Brown", "Mechanical", 8.25);  // RNo 103
                    studentDB_agb.insert_agb(104, "Diana Wilson", "Civil", 9.10);        // RNo 104
                    studentDB_agb.insert_agb(105, "Eve Davis", "Computer Science", 8.85);      // RNo 105
                    studentDB_agb.insert_agb(106, "Frank Miller", "Electronics", 7.65);    // RNo 106
                    studentDB_agb.insert_agb(107, "Grace Lee", "Mechanical", 8.40);       // RNo 107
                    studentDB_agb.insert_agb(108, "Henry Moore", "Civil", 9.25);         // RNo 108
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
===== Student Records Management System =====
Enter hash table size: 10
Student Hash Table created with capacity: 10

===== Student Database Menu =====
5. Insert sample data
Enter your choice: 5
Inserting sample student records...

Inserting Student Roll No 101...
Hash value: 1
✓ Student Alice Johnson (Roll No: 101) inserted successfully!

Inserting Student Roll No 102...
Hash value: 2
✓ Student Bob Smith (Roll No: 102) inserted successfully!

Inserting Student Roll No 103...
Hash value: 3
✓ Student Charlie Brown (Roll No: 103) inserted successfully!

Inserting Student Roll No 104...
Hash value: 4
✓ Student Diana Wilson (Roll No: 104) inserted successfully!

Inserting Student Roll No 105...
Hash value: 5
✓ Student Eve Davis (Roll No: 105) inserted successfully!

Inserting Student Roll No 106...
Hash value: 6
✓ Student Frank Miller (Roll No: 106) inserted successfully!

Inserting Student Roll No 107...
Hash value: 7
✓ Student Grace Lee (Roll No: 107) inserted successfully!

Inserting Student Roll No 108...
Hash value: 8
✓ Student Henry Moore (Roll No: 108) inserted successfully!

===== Student Database Menu =====
4. Display all student records
Enter your choice: 4

========== Student Records ==========
Roll No:  101, Name:   Alice Johnson, Branch: Computer Science, CGPA: 8.75
Roll No:  102, Name:       Bob Smith, Branch:     Electronics, CGPA: 7.90
Roll No:  103, Name:  Charlie Brown, Branch:      Mechanical, CGPA: 8.25
Roll No:  104, Name:   Diana Wilson, Branch:           Civil, CGPA: 9.10
Roll No:  105, Name:       Eve Davis, Branch: Computer Science, CGPA: 8.85
Roll No:  106, Name:   Frank Miller, Branch:     Electronics, CGPA: 7.65
Roll No:  107, Name:      Grace Lee, Branch:      Mechanical, CGPA: 8.40
Roll No:  108, Name:    Henry Moore, Branch:           Civil, CGPA: 9.25

--- Statistics ---
Total students: 8
Table capacity: 10
Load factor: 0.80
==================================

===== Student Database Menu =====
2. Search student by roll number
Enter your choice: 2
Enter Roll Number to search: 105

Searching for Student Roll No 105...
Hash value: 5
✓ Student found!

--- Student Details ---
Roll No: 105
Name: Eve Davis
Branch: Computer Science
CGPA: 8.85

===== Student Database Menu =====
3. Delete student record
Enter your choice: 3
Enter Roll Number to delete: 103

Deleting Student Roll No 103...
Hash value: 3
✓ Student with Roll No 103 deleted successfully!

===== Student Database Menu =====
4. Display all student records
Enter your choice: 4

========== Student Records ==========
Roll No:  101, Name:   Alice Johnson, Branch: Computer Science, CGPA: 8.75
Roll No:  102, Name:       Bob Smith, Branch:     Electronics, CGPA: 7.90
Roll No:  104, Name:   Diana Wilson, Branch:           Civil, CGPA: 9.10
Roll No:  105, Name:       Eve Davis, Branch: Computer Science, CGPA: 8.85
Roll No:  106, Name:   Frank Miller, Branch:     Electronics, CGPA: 7.65
Roll No:  107, Name:      Grace Lee, Branch:      Mechanical, CGPA: 8.40
Roll No:  108, Name:    Henry Moore, Branch:           Civil, CGPA: 9.25

--- Statistics ---
Total students: 7
Table capacity: 10
Load factor: 0.70
==================================

===== Student Database Menu =====
6. Exit
Enter your choice: 6
Exiting... Thank you!
Student Hash Table memory deallocated
```

---

## Dry Run

### Hash Table Size: 10, Inserting: 101, 102, 103, 104, 105, 106, 107, 108

### Initial State:

```
Index:  0   1   2   3   4   5   6   7   8   9
Entry: [-] [-] [-] [-] [-] [-] [-] [-] [-] [-]
       Empty slots
```

### Insert 101 (hash = 101 % 10 = 1):

```
Index:  0    1         2   3   4   5   6   7   8   9
Entry: [-] [101:Alice] [-] [-] [-] [-] [-] [-] [-] [-]
```

### Insert 102 (hash = 102 % 10 = 2):

```
Index:  0    1         2        3   4   5   6   7   8   9
Entry: [-] [101:Alice] [102:Bob] [-] [-] [-] [-] [-] [-] [-]
```

### Insert 103 (hash = 103 % 10 = 3):

```
Index:  0    1         2        3              4   5   6   7   8   9
Entry: [-] [101:Alice] [102:Bob] [103:Charlie] [-] [-] [-] [-] [-] [-]
```

### Insert 104 (hash = 104 % 10 = 4):

```
Index:  0    1         2        3              4             5   6   7   8   9
Entry: [-] [101:Alice] [102:Bob] [103:Charlie] [104:Diana] [-] [-] [-] [-] [-]
```

### Insert 105 (hash = 105 % 10 = 5):

```
Index:  0    1         2        3              4             5          6   7   8   9
Entry: [-] [101:Alice] [102:Bob] [103:Charlie] [104:Diana] [105:Eve] [-] [-] [-] [-]
```

### Insert 106 (hash = 106 % 10 = 6):

```
Index:  0    1         2        3              4             5          6              7   8   9
Entry: [-] [101:Alice] [102:Bob] [103:Charlie] [104:Diana] [105:Eve] [106:Frank] [-] [-] [-]
```

### Insert 107 (hash = 107 % 10 = 7):

```
Index:  0    1         2        3              4             5          6              7           8   9
Entry: [-] [101:Alice] [102:Bob] [103:Charlie] [104:Diana] [105:Eve] [106:Frank] [107:Grace] [-] [-]
```

### Insert 108 (hash = 108 % 10 = 8):

```
Index:  0    1         2        3              4             5          6              7           8            9
Entry: [-] [101:Alice] [102:Bob] [103:Charlie] [104:Diana] [105:Eve] [106:Frank] [107:Grace] [108:Henry] [-]
```

### Search for 105:

```
Hash(105) = 105 % 10 = 5
Check index 5: Roll No 105 = 105 ✓ FOUND
```

### Delete 103:

```
Hash(103) = 103 % 10 = 3
Check index 3: Roll No 103 = 103 ✓ FOUND
Mark index 3 as empty

Index:  0    1         2        3   4             5          6              7           8            9
Entry: [-] [101:Alice] [102:Bob] [-] [104:Diana] [105:Eve] [106:Frank] [107:Grace] [108:Henry] [-]
```

---

## Conclusion

This program successfully implements a **Student Records Management System using Hashing with Linear Probing**:

1. **Hash Table with Linear Probing**:

   - Uses roll number as the key for hashing
   - Resolves collisions using linear probing technique
   - Efficient storage and retrieval of student records

2. **Key Features**:

   - Student records with roll number, name, branch, and CGPA
   - Insert, search, delete, and display operations
   - Load factor calculation for performance monitoring
   - Duplicate prevention

3. **Linear Probing Technique**:

   - When a collision occurs, checks the next slot sequentially
   - Wraps around to the beginning if reaching the end
   - Simple and cache-friendly collision resolution

4. **Time Complexity**:

   - **Best Case**: O(1) - No collisions
   - **Average Case**: O(1) with low load factor
   - **Worst Case**: O(n) - All elements form a cluster

5. **Space Complexity**: O(n) where n is the table size

6. **Advantages**:

   - **Cache Performance**: Sequential memory access
   - **No Extra Memory**: No need for pointers like chaining
   - **Simple Implementation**: Easy to understand and implement

7. **Disadvantages**:
   - **Primary Clustering**: Consecutive filled slots form clusters
   - **Deletion Complexity**: Requires special handling with markers
   - **Load Factor Limitation**: Performance degrades with high load factor

**Applications**:

- Student database systems
- Employee record management
- Library management systems
- Inventory management
- Any system requiring fast key-based lookups

**Best Practices**:

- Keep load factor below 0.7 for good performance
- Use prime number for table size
- Implement proper deletion with lazy deletion markers
- Monitor and resize table when load factor becomes too high
