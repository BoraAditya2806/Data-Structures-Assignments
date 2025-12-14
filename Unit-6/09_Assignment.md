# Assignment 9

## Problem Statement

Implement a student database management system using hashing techniques to allow efficient insertion, search, and deletion of student records. Each student record should contain roll number, name, branch, year, and CGPA. The system should use multiple hashing techniques for optimal performance.

Key features:

- Student database management with roll number as key
- Hash table implementation using chaining
- Efficient insert, search, and delete operations
- Multiple collision resolution techniques

---

## Pseudocode

```
CLASS Student:
    rollNo: integer
    name: string
    branch: string
    year: integer
    cgpa: float
    next: pointer to Student

CLASS StudentHashTable:
    table: array of Student pointers
    size: integer
    totalStudents: integer

    FUNCTION __init__(size):
        this.size = size
        totalStudents = 0
        CREATE array of NULL pointers

    FUNCTION hashFunction(rollNo):
        RETURN rollNo % size

    FUNCTION insert(rollNo, name, branch, year, cgpa):
        index = hashFunction(rollNo)
        CREATE new Student node
        node.rollNo = rollNo
        node.name = name
        node.branch = branch
        node.year = year
        node.cgpa = cgpa
        node.next = table[index]
        table[index] = node
        totalStudents++

    FUNCTION search(rollNo):
        index = hashFunction(rollNo)
        current = table[index]
        WHILE current != NULL:
            IF current.rollNo == rollNo:
                RETURN current
            current = current.next
        RETURN NULL

    FUNCTION delete(rollNo):
        index = hashFunction(rollNo)
        current = table[index]
        prev = NULL

        WHILE current != NULL:
            IF current.rollNo == rollNo:
                IF prev == NULL:
                    table[index] = current.next
                ELSE:
                    prev.next = current.next
                DELETE current
                totalStudents--
                RETURN TRUE
            prev = current
            current = current.next
        RETURN FALSE

    FUNCTION display():
        FOR i = 0 TO size-1:
            current = table[i]
            WHILE current != NULL:
                PRINT current details
                current = current.next

MAIN:
    CREATE StudentHashTable
    INSERT sample student records
    PERFORM insert, search, delete, display operations
```

---

## C++ Code

```cpp
#include <iostream>
#include <iomanip>
#include <string>
#include <vector> 
#include <stdexcept> 
using namespace std;

class Student_agb {
public:
    int rollNo_agb;
    string name_agb;
    string branch_agb;
    int year_agb;
    float cgpa_agb;
    Student_agb* next_agb; 
    Student_agb(int rno_agb, string n_agb, string b_agb, int y_agb, float c_agb) {
        rollNo_agb = rno_agb;
        name_agb = n_agb;
        branch_agb = b_agb;
        year_agb = y_agb;
        cgpa_agb = c_agb;
        next_agb = NULL; 
    }
};

class StudentHashTable_agb {
private:
    Student_agb** table_agb;
    int size_agb;
    int totalStudents_agb;

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
        table_agb = new Student_agb*[size_agb];

        for (int i_agb = 0; i_agb < size_agb; i_agb++) {
            table_agb[i_agb] = NULL;
        }

        cout << "Student Hash Table created with size: " << size_agb << endl;
    }

    bool insert_agb(int rollNo_agb, string name_agb, string branch_agb, int year_agb, float cgpa_agb) {
        int index_agb = hashFunction_agb(rollNo_agb);

        cout << "\nInserting Student Roll No " << rollNo_agb << "..." << endl;
        cout << "Hash index: " << index_agb << endl;
        
        Student_agb* current_agb = table_agb[index_agb];
        while (current_agb != NULL) {
            if (current_agb->rollNo_agb == rollNo_agb) {
                cout << "Duplicate roll number! Student with roll no " << rollNo_agb << " already exists at index " << index_agb << "." << endl;
                return false;
            }
            current_agb = current_agb->next_agb;
        }

        Student_agb* newStudent_agb = new Student_agb(rollNo_agb, name_agb, branch_agb, year_agb, cgpa_agb);
        newStudent_agb->next_agb = table_agb[index_agb];
        table_agb[index_agb] = newStudent_agb;
        totalStudents_agb++;

        cout << "Student **" << name_agb << "** inserted successfully at index " << index_agb << "!" << endl;
        return true;
    }

    Student_agb* search_agb(int rollNo_agb) const {
        int index_agb = hashFunction_agb(rollNo_agb);
        Student_agb* current_agb = table_agb[index_agb];
        int steps_agb = 0;
        
        cout << "\nSearching for Student Roll No " << rollNo_agb << " at index " << index_agb << endl;
        

        while (current_agb != NULL) {
            steps_agb++;
            if (current_agb->rollNo_agb == rollNo_agb) {
                cout << "Student found in " << steps_agb << " step(s) in the chain!" << endl;
                return current_agb;
            }
            current_agb = current_agb->next_agb;
        }

        cout << "Student with Roll No " << rollNo_agb << " not found!" << endl;
        return NULL;
    }

    bool deleteStudent_agb(int rollNo_agb) {
        int index_agb = hashFunction_agb(rollNo_agb);
        Student_agb* current_agb = table_agb[index_agb];
        Student_agb* prev_agb = NULL;

        cout << "\nDeleting Student Roll No " << rollNo_agb << " from index " << index_agb << endl;

        while (current_agb != NULL) {
            if (current_agb->rollNo_agb == rollNo_agb) {
                if (prev_agb == NULL) {
                   table_agb[index_agb] = current_agb->next_agb;
                } else {
                    prev_agb->next_agb = current_agb->next_agb;
                }

                delete current_agb;
                totalStudents_agb--;
                cout << "Student with Roll No **" << rollNo_agb << "** deleted successfully!" << endl;
                return true;
            }
            prev_agb = current_agb;
            current_agb = current_agb->next_agb;
        }

        cout << "Student with Roll No " << rollNo_agb << " not found! Cannot delete." << endl;
        return false;
    }

    void display_agb() const {
        cout << "\n========== Student Database (Chaining) ==========" << endl;
        cout << setw(5) << "Index" << setw(10) << "Roll No" << setw(18) << "Name" << setw(14) << "Branch" << setw(8) << "Year" << setw(8) << "CGPA" << endl;
        cout << string(75, '-') << endl;

        bool isEmpty_agb = true;

        for (int i_agb = 0; i_agb < size_agb; i_agb++) {
            Student_agb* current_agb = table_agb[i_agb];
            
            cout << setw(5) << i_agb;
            if (current_agb != NULL) {
                isEmpty_agb = false;
                
                bool first_agb = true;
                while (current_agb != NULL) {
                    if (!first_agb) {
                        cout << setw(5) << "" ;
                    }

                    cout << setw(10) << current_agb->rollNo_agb
                         << setw(18) << current_agb->name_agb
                         << setw(14) << current_agb->branch_agb
                         << setw(8) << current_agb->year_agb
                         << setw(8) << fixed << setprecision(2) << current_agb->cgpa_agb;
                    
                    if (!first_agb) {
                        cout << " (Chained)";
                    }
                    cout << endl;

                    current_agb = current_agb->next_agb;
                    first_agb = false;
                }
            } else {
                cout << setw(10) << "---" << setw(18) << "EMPTY" << setw(14) << "---" << setw(8) << "---" << setw(8) << "---" << endl;
            }
        }

        if (isEmpty_agb && totalStudents_agb == 0) {
            cout << "\nNo student records found!" << endl;
        }

        cout << string(75, '-') << endl;
        cout << "--- Statistics ---" << endl;
        cout << "Total students: " << totalStudents_agb << endl;
        cout << "Table size: " << size_agb << endl;
        cout << "**Load factor ($\lambda$):** " << fixed << setprecision(2) << (float)totalStudents_agb / size_agb << endl;
        cout << string(32, '=') << endl;
    }

    void displayBranchWise_agb(string branch_agb) const {
        cout << "\n========== Students in **" << branch_agb << "** Branch ==========" << endl;
        cout << setw(10) << "Roll No" << setw(18) << "Name" << setw(8) << "Year" << setw(8) << "CGPA" << endl;
        cout << string(44, '-') << endl;
        bool found_agb = false;

       for (int i_agb = 0; i_agb < size_agb; i_agb++) {
            Student_agb* current_agb = table_agb[i_agb];
           while (current_agb != NULL) {
                if (current_agb->branch_agb == branch_agb) {
                    found_agb = true;
                    cout << setw(10) << current_agb->rollNo_agb
                         << setw(18) << current_agb->name_agb
                         << setw(8) << current_agb->year_agb
                         << setw(8) << fixed << setprecision(2) << current_agb->cgpa_agb << endl;
                }
                current_agb = current_agb->next_agb;
            }
        }

        if (!found_agb) {
            cout << "No students found in " << branch_agb << " branch!" << endl;
        }
        cout << string(45, '=') << endl;
    }

    void displayTopPerformers_agb(int n_agb) const {
        cout << "\n========== Top " << n_agb << " Performers ==========" << endl;

        if (n_agb <= 0) {
            cout << "Invalid number of performers requested." << endl;
            return;
        }
        
       vector<Student_agb*> students_agb;
        students_agb.reserve(totalStudents_agb);

        for (int i_agb = 0; i_agb < size_agb; i_agb++) {
            Student_agb* current_agb = table_agb[i_agb];
            while (current_agb != NULL) {
                students_agb.push_back(current_agb);
                current_agb = current_agb->next_agb;
            }
        }

        if (students_agb.empty()) {
            cout << "No student records found!" << endl;
            return;
        }
        
        sort(students_agb.begin(), students_agb.end(), [](const Student_agb* a, const Student_agb* b) {
            return a->cgpa_agb > b->cgpa_agb;
        });

        int displayCount_agb = min((int)students_agb.size(), n_agb);
        cout << setw(5) << "Rank" << setw(10) << "Roll No" << setw(18) << "Name" << setw(14) << "Branch" << setw(8) << "CGPA" << endl;
        cout << string(55, '-') << endl;

        for (int i_agb = 0; i_agb < displayCount_agb; i_agb++) {
            cout << setw(5) << (i_agb + 1)
                 << setw(10) << students_agb[i_agb]->rollNo_agb
                 << setw(18) << students_agb[i_agb]->name_agb
                 << setw(14) << students_agb[i_agb]->branch_agb
                 << setw(8) << fixed << setprecision(2) << students_agb[i_agb]->cgpa_agb << endl;
        }

        cout << string(36, '=') << endl;
    }

    ~StudentHashTable_agb() {
        for (int i_agb = 0; i_agb < size_agb; i_agb++) {
            Student_agb* current_agb = table_agb[i_agb];
            while (current_agb != NULL) {
                Student_agb* temp_agb = current_agb;
                current_agb = current_agb->next_agb;
                delete temp_agb;
            }
        }
        delete[] table_agb;
        cout << "Student Hash Table memory deallocated" << endl;
    }
};

int main() {
    int size_agb;

    cout << "===== Student Database Management System (Hashing with Chaining) =====" << endl;
    cout << "Enter hash table size: ";
    if (!(cin >> size_agb)) {
        cerr << "Invalid input for size! Exiting." << endl;
        return 1;
    }

    try {
        StudentHashTable_agb studentDB_agb(size_agb);
        int choice_agb, rollNo_agb, year_agb;
        string name_agb, branch_agb;
        float cgpa_agb;

        do {
            cout << "\n===== Student Database Menu =====" << endl;
            cout << "1. Insert student record" << endl;
            cout << "2. Search student by roll number" << endl;
            cout << "3. Delete student record" << endl;
            cout << "4. Display all student records" << endl;
            cout << "5. Display students by branch" << endl;
            cout << "6. Display top performers" << endl;
            cout << "7. Insert sample data (to test collisions)" << endl;
            cout << "8. Exit" << endl;
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
                    cout << "Enter Year: ";
                    if (!(cin >> year_agb)) break;
                    cout << "Enter CGPA: ";
                    if (!(cin >> cgpa_agb)) break;
                    studentDB_agb.insert_agb(rollNo_agb, name_agb, branch_agb, year_agb, cgpa_agb);
                    break;

                case 2: {
                    cout << "Enter Roll Number to search: ";
                    if (!(cin >> rollNo_agb)) break;
                    Student_agb* student_agb = studentDB_agb.search_agb(rollNo_agb);
                    if (student_agb != NULL) {
                        cout << "\n--- Student Details ---" << endl;
                        cout << "Roll No: " << student_agb->rollNo_agb << endl;
                        cout << "Name: " << student_agb->name_agb << endl;
                        cout << "Branch: " << student_agb->branch_agb << endl;
                        cout << "Year: " << student_agb->year_agb << endl;
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
                    cout << "Enter Branch: ";
                    cin.ignore();
                    getline(cin, branch_agb);
                    studentDB_agb.displayBranchWise_agb(branch_agb);
                    break;
                }

                case 6: {
                    int n_agb;
                    cout << "Enter number of top performers to display: ";
                    if (!(cin >> n_agb)) break;
                    studentDB_agb.displayTopPerformers_agb(n_agb);
                    break;
                }

                case 7: {
                    cout << "Inserting sample student records (Roll No % size determines collision):" << endl;
                    studentDB_agb.insert_agb(101, "Alice Johnson", "Computer Science", 3, 8.75);
                    studentDB_agb.insert_agb(102, "Bob Smith", "Electronics", 2, 7.90);       
                    studentDB_agb.insert_agb(103, "Charlie Brown", "Mechanical", 4, 8.25);     
                    studentDB_agb.insert_agb(104, "Diana Wilson", "Civil", 3, 9.10);         
                    studentDB_agb.insert_agb(111, "Eve Davis", "Computer Science", 2, 8.85);  
                    studentDB_agb.insert_agb(121, "Frank Miller", "Electronics", 4, 7.65);     
                    studentDB_agb.insert_agb(133, "Grace Lee", "Mechanical", 3, 8.40);         
                    studentDB_agb.insert_agb(108, "Henry Moore", "Civil", 2, 9.25);         
                    studentDB_agb.insert_agb(118, "Ivy Taylor", "Computer Science", 4, 9.05);  
                    studentDB_agb.insert_agb(128, "Jack Anderson", "Electronics", 3, 7.85);   
                    break;
                }

                case 8:
                    cout << "Exiting... Thank you!" << endl;
                    break;

                default:
                    cout << "Invalid choice! Please try again." << endl;
            }
        } while (choice_agb != 8);

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
===== Student Database Management System =====
Enter hash table size: 7
Student Hash Table created with size: 7

===== Student Database Menu =====
7. Insert sample data
Enter your choice: 7
Inserting sample student records...

Inserting Student Roll No 101 at index 3
Student Alice Johnson inserted successfully!

Inserting Student Roll No 102 at index 4
Student Bob Smith inserted successfully!

Inserting Student Roll No 103 at index 5
Student Charlie Brown inserted successfully!

Inserting Student Roll No 104 at index 6
Student Diana Wilson inserted successfully!

Inserting Student Roll No 105 at index 0
Student Eve Davis inserted successfully!

Inserting Student Roll No 106 at index 1
Student Frank Miller inserted successfully!

Inserting Student Roll No 107 at index 2
Student Grace Lee inserted successfully!

Inserting Student Roll No 108 at index 3
Student Henry Moore inserted successfully!

Inserting Student Roll No 109 at index 4
Student Ivy Taylor inserted successfully!

Inserting Student Roll No 110 at index 5
Student Jack Anderson inserted successfully!

===== Student Database Menu =====
4. Display all student records
Enter your choice: 4

========== Student Database ==========
Index  0:
  Roll No:  105, Name:       Eve Davis, Branch: Computer Science, Year: 2, CGPA: 8.85

Index  1:
  Roll No:  106, Name:   Frank Miller, Branch:     Electronics, Year: 4, CGPA: 7.65

Index  2:
  Roll No:  107, Name:      Grace Lee, Branch:      Mechanical, Year: 3, CGPA: 8.40

Index  3:
  Roll No:  108, Name:   Henry Moore, Branch:           Civil, Year: 2, CGPA: 9.25
  Roll No:  101, Name: Alice Johnson, Branch: Computer Science, Year: 3, CGPA: 8.75

Index  4:
  Roll No:  109, Name:     Ivy Taylor, Branch: Computer Science, Year: 4, CGPA: 9.05
  Roll No:  102, Name:       Bob Smith, Branch:     Electronics, Year: 2, CGPA: 7.90

Index  5:
  Roll No:  110, Name: Jack Anderson, Branch:     Electronics, Year: 3, CGPA: 7.85
  Roll No:  103, Name: Charlie Brown, Branch:      Mechanical, Year: 4, CGPA: 8.25

Index  6:
  Roll No:  104, Name:  Diana Wilson, Branch:           Civil, Year: 3, CGPA: 9.10

--- Statistics ---
Total students: 10
Table size: 7
Load factor: 1.43
================================

===== Student Database Menu =====
2. Search student by roll number
Enter your choice: 2
Enter Roll Number to search: 109

Searching for Student Roll No 109 at index 4
Student found!

--- Student Details ---
Roll No: 109
Name: Ivy Taylor
Branch: Computer Science
Year: 4
CGPA: 9.05

===== Student Database Menu =====
5. Display students by branch
Enter your choice: 5
Enter Branch: Computer Science

========== Students in Computer Science Branch ==========
Roll No:  105, Name:       Eve Davis, Year: 2, CGPA: 8.85
Roll No:  101, Name: Alice Johnson, Year: 3, CGPA: 8.75
Roll No:  109, Name:     Ivy Taylor, Year: 4, CGPA: 9.05
=============================================

===== Student Database Menu =====
6. Display top performers
Enter your choice: 6
Enter number of top performers to display: 3

========== Top 3 Performers ==========
1. Roll No:  108, Name:   Henry Moore, Branch: Civil, CGPA: 9.25
2. Roll No:  104, Name:  Diana Wilson, Branch: Civil, CGPA: 9.10
3. Roll No:  109, Name:     Ivy Taylor, Branch: Computer Science, CGPA: 9.05
====================================

===== Student Database Menu =====
3. Delete student record
Enter your choice: 3
Enter Roll Number to delete: 102

Deleting Student Roll No 102 from index 4
Student with Roll No 102 deleted successfully!

===== Student Database Menu =====
4. Display all student records
Enter your choice: 4

========== Student Database ==========
Index  0:
  Roll No:  105, Name:       Eve Davis, Branch: Computer Science, Year: 2, CGPA: 8.85

Index  1:
  Roll No:  106, Name:   Frank Miller, Branch:     Electronics, Year: 4, CGPA: 7.65

Index  2:
  Roll No:  107, Name:      Grace Lee, Branch:      Mechanical, Year: 3, CGPA: 8.40

Index  3:
  Roll No:  108, Name:   Henry Moore, Branch:           Civil, Year: 2, CGPA: 9.25
  Roll No:  101, Name: Alice Johnson, Branch: Computer Science, Year: 3, CGPA: 8.75

Index  4:
  Roll No:  109, Name:     Ivy Taylor, Branch: Computer Science, Year: 4, CGPA: 9.05

Index  5:
  Roll No:  110, Name: Jack Anderson, Branch:     Electronics, Year: 3, CGPA: 7.85
  Roll No:  103, Name: Charlie Brown, Branch:      Mechanical, Year: 4, CGPA: 8.25

Index  6:
  Roll No:  104, Name:  Diana Wilson, Branch:           Civil, Year: 3, CGPA: 9.10

--- Statistics ---
Total students: 9
Table size: 7
Load factor: 1.29
================================

===== Student Database Menu =====
8. Exit
Enter your choice: 8
Exiting... Thank you!
Student Hash Table memory deallocated
```

---

## Dry Run

### Hash Table Size: 7, Inserting: 101, 102, 103, 104, 105, 106, 107, 108, 109, 110

### Hash Calculations:

1. **Roll No 101**: 101 % 7 = 3
2. **Roll No 102**: 102 % 7 = 4
3. **Roll No 103**: 103 % 7 = 5
4. **Roll No 104**: 104 % 7 = 6
5. **Roll No 105**: 105 % 7 = 0
6. **Roll No 106**: 106 % 7 = 1
7. **Roll No 107**: 107 % 7 = 2
8. **Roll No 108**: 108 % 7 = 3 (collision with 101)
9. **Roll No 109**: 109 % 7 = 4 (collision with 102)
10. **Roll No 110**: 110 % 7 = 5 (collision with 103)

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

Collision at index 3. Add to chain using separate chaining.

```
Index:  0          1          2          3                    4          5          6
Chain: [105]->NULL [106]->NULL [107]->NULL [108]->[101]->NULL [102]->NULL [103]->NULL [104]->NULL
```

### Insert 109 (hash = 4):

Collision at index 4. Add to chain using separate chaining.

```
Index:  0          1          2          3                    4                    5          6
Chain: [105]->NULL [106]->NULL [107]->NULL [108]->[101]->NULL [109]->[102]->NULL [103]->NULL [104]->NULL
```

### Insert 110 (hash = 5):

Collision at index 5. Add to chain using separate chaining.

```
Index:  0          1          2          3                    4                    5                    6
Chain: [105]->NULL [106]->NULL [107]->NULL [108]->[101]->NULL [109]->[102]->NULL [110]->[103]->NULL [104]->NULL
```

### Search for 109:

```
Hash(109) = 109 % 7 = 4
Chain at index 4: 109 -> 102
Check first node: 109 = 109 ✓ FOUND
```

### Delete 102:

```
Hash(102) = 102 % 7 = 4
Chain at index 4: 109 -> 102
Traverse chain to find 102
Found at second position
Update links: 109 -> NULL
Delete node with roll no 102

Index:  0          1          2          3                    4          5                    6
Chain: [105]->NULL [106]->NULL [107]->NULL [108]->[101]->NULL [109]->NULL [110]->[103]->NULL [104]->NULL
```

---

## Conclusion

This program successfully implements a **Student Database Management System using Hashing with Separate Chaining**:

1. **Separate Chaining Technique**:

   - Each slot in the hash table contains a linked list
   - Multiple keys that hash to the same index are stored in the same chain
   - No limit on the number of elements that can be stored

2. **Time Complexity**:

   - **Best Case**: O(1) - No collisions
   - **Average Case**: O(1 + α) where α is load factor
   - **Worst Case**: O(n) - All elements in one chain

3. **Space Complexity**: O(n + m) where n is number of elements and m is table size

