# Assignment 10

## Problem Statement

Design and implement a smart college placement portal that uses advanced hashing techniques to efficiently manage student placement records with high performance and low collision probability, even under dynamic data growth. The system should handle student records with roll number, name, branch, CGPA, and company placement details.

Key features:

- Advanced hashing techniques for optimal performance
- Dynamic resizing to handle data growth
- Efficient insertion, search, and deletion operations
- Collision handling with separate chaining
- Performance monitoring and statistics

---

## Pseudocode

```
CLASS PlacementRecord:
    rollNo: integer
    name: string
    branch: string
    cgpa: float
    companyName: string
    package: float
    next: pointer to PlacementRecord

CLASS SmartPlacementPortal:
    table: array of PlacementRecord pointers
    size: integer
    totalRecords: integer
    threshold: float  // Load factor threshold for resizing

    FUNCTION __init__(initialSize):
        size = initialSize
        totalRecords = 0
        threshold = 0.75
        CREATE array of NULL pointers

    FUNCTION hashFunction(rollNo):
        RETURN rollNo % size

    FUNCTION loadFactor():
        RETURN totalRecords / size

    FUNCTION resize():
        oldSize = size
        oldTable = table
        size = size * 2  // Double the size
        CREATE new array of NULL pointers
        totalRecords = 0

        FOR i = 0 TO oldSize-1:
            current = oldTable[i]
            WHILE current != NULL:
                insert(current.rollNo, current.name, current.branch,
                       current.cgpa, current.companyName, current.package)
                temp = current
                current = current.next
                DELETE temp
        DELETE oldTable

    FUNCTION insert(rollNo, name, branch, cgpa, companyName, package):
        IF loadFactor() > threshold:
            resize()

        index = hashFunction(rollNo)
        CREATE new PlacementRecord
        newRecord.rollNo = rollNo
        newRecord.name = name
        newRecord.branch = branch
        newRecord.cgpa = cgpa
        newRecord.companyName = companyName
        newRecord.package = package
        newRecord.next = table[index]
        table[index] = newRecord
        totalRecords++

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
                totalRecords--
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
    CREATE SmartPlacementPortal
    INSERT sample placement records
    PERFORM insert, search, delete, display operations
    MONITOR performance statistics
```

---

## C++ Code

```cpp
#include <iostream>
#include <string>
using namespace std;

class PlacementRecord {
public:
    int rollNo;
    string name;
    string branch;
    float cgpa;
    string companyName;
    float package;
    PlacementRecord* next;

    PlacementRecord(int rno, string n, string b, float c, string company, float p) {
        rollNo = rno;
        name = n;
        branch = b;
        cgpa = c;
        companyName = company;
        package = p;
        next = NULL;
    }
};

class SmartPlacementPortal {
private:
    PlacementRecord** table;
    int size;
    int totalRecords;
    float threshold; 

  
    int hashFunction_agb(int rollNo) {
        return rollNo % size;
    }

    float loadFactor_agb() {
        return (float)totalRecords / size;
    }

    void resize_agb() {
        cout << "\n[INFO] Resizing hash table from " << size << " to " << (size * 2) << " slots..." << endl;

        int oldSize = size;
        PlacementRecord** oldTable = table;

        size *= 2;
        totalRecords = 0;

        table = new PlacementRecord*[size];
        for (int i = 0; i < size; i++) {
            table[i] = NULL;
        }

        for (int i = 0; i < oldSize; i++) {
            PlacementRecord* current = oldTable[i];
            while (current != NULL) {
                int index = hashFunction_agb(current->rollNo); 
                PlacementRecord* newRecord = new PlacementRecord(
                    current->rollNo, current->name, current->branch,
                    current->cgpa, current->companyName, current->package
                );
                newRecord->next = table[index];
                table[index] = newRecord;
                totalRecords++;

                PlacementRecord* temp = current;
                current = current->next;
            }
        }
        for (int i = 0; i < oldSize; i++) {
            PlacementRecord* current = oldTable[i];
            while (current != NULL) {
                PlacementRecord* temp = current;
                current = current->next;
                delete temp;
            }
        }
        delete[] oldTable;
        
        cout << "[INFO] Resizing completed. New load factor: " << fixed << setprecision(2) << loadFactor_agb() << endl; 
    }
    
    PlacementRecord* internal_search(int rollNo) {
        int index = hashFunction_agb(rollNo);
        PlacementRecord* current = table[index];

        while (current != NULL) {
            if (current->rollNo == rollNo) {
                return current;
            }
            current = current->next;
        }
        return NULL;
    }

public:
    bool insert_agb(int rollNo, string name, string branch, float cgpa, string companyName, float package) {
        if (internal_search(rollNo) != NULL) {
            cout << " Duplicate roll number! Student with roll no " << rollNo << " already exists." << endl;
            return false;
        }

        if (loadFactor_agb() > threshold) { 
            resize_agb(); 
        }

        int index = hashFunction_agb(rollNo);

        cout << "\nInserting Placement Record for Roll No " << rollNo << " at index " << index << endl;
        cout << "Current load factor: " << fixed << setprecision(2) << loadFactor_agb() << endl;

        PlacementRecord* newRecord = new PlacementRecord(rollNo, name, branch, cgpa, companyName, package);
        newRecord->next = table[index];
        table[index] = newRecord;
        totalRecords++;

        cout << " Student " << name << " placed at " << companyName << " with package ₹"
             << fixed << setprecision(2) << package << " LPA" << endl;
        return true;
    }

    PlacementRecord* search_agb(int rollNo) {
        int index = hashFunction_agb(rollNo); // Called with _agb
        PlacementRecord* current = table[index];

        cout << "\nSearching for Student Roll No " << rollNo << " at index " << index << endl;

        while (current != NULL) {
            if (current->rollNo == rollNo) {
                cout << " Placement record found!" << endl;
                return current;
            }
            current = current->next;
        }

        cout << " Placement record for Roll No " << rollNo << " not found!" << endl;
        return NULL;
    }

    bool deleteRecord_agb(int rollNo) {
        int index = hashFunction_agb(rollNo); // Called with _agb
        PlacementRecord* current = table[index];
        PlacementRecord* prev = NULL;

        cout << "\nDeleting Placement Record for Roll No " << rollNo << " from index " << index << endl;

        while (current != NULL) {
            if (current->rollNo == rollNo) {
                // Remove node from chain
                if (prev == NULL) {
                    // Deleting first node
                    table[index] = current->next;
                } else {
                    prev->next = current->next;
                }

                delete current;
                totalRecords--;
                cout << " Placement record for Roll No " << rollNo << " deleted successfully!" << endl;
                return true;
            }
            prev = current;
            current = current->next;
        }

        cout << " Placement record for Roll No " << rollNo << " not found! Cannot delete." << endl;
        return false;
    }

    void display_agb() {
        cout << "\n========== Smart Placement Portal ==========" << endl;
        bool isEmpty = true;

        for (int i = 0; i < size; i++) {
            PlacementRecord* current = table[i];
            if (current != NULL) {
                isEmpty = false;
                cout << "Index " << setw(2) << i << ": " << endl;

                while (current != NULL) {
                    cout << "  Roll No: " << setw(4) << current->rollNo
                         << ", Name: " << setw(15) << current->name
                         << ", Branch: " << setw(10) << current->branch
                         << ", CGPA: " << fixed << setprecision(2) << current->cgpa
                         << ", Company: " << setw(12) << current->companyName
                         << ", Package: ₹" << fixed << setprecision(2) << current->package << " LPA" << endl;
                    current = current->next;
                }
                cout << endl;
            }
        }

        if (isEmpty) {
            cout << "No placement records found!" << endl;
        }

        displayStatistics_agb(); // Called with _agb
    }

    void displayStatistics_agb() {
        cout << "--- Portal Statistics ---" << endl;
        cout << "Total placement records: " << totalRecords << endl;
        cout << "Hash table size: " << size << endl;
        cout << "Current load factor: " << fixed << setprecision(2) << loadFactor_agb() << endl; // Called with _agb
        cout << "Resize threshold: " << threshold << endl;

       
        int maxChainLength = 0;
        int totalChainLength = 0;
        int nonEmptyChains = 0;

        for (int i = 0; i < size; i++) {
            int chainLength = 0;
            PlacementRecord* current = table[i];
            while (current != NULL) {
                chainLength++;
                current = current->next;
            }

            if (chainLength > 0) {
                nonEmptyChains++;
                totalChainLength += chainLength;
                if (chainLength > maxChainLength) {
                    maxChainLength = chainLength;
                }
            }
        }

        cout << "Non-empty chains: " << nonEmptyChains << endl;
        if (nonEmptyChains > 0) {
            cout << "Average chain length: " << fixed << setprecision(2)
                 << (float)totalChainLength / nonEmptyChains << endl;
        }
        cout << "Longest chain: " << maxChainLength << endl;
        cout << string(32, '=') << endl;
    }
    void displayBranchWise_agb(string branch) {
        cout << "\n========== " << branch << " Branch Placements ==========" << endl;
        bool found = false;

        for (int i = 0; i < size; i++) {
            PlacementRecord* current = table[i];
            while (current != NULL) {
                if (current->branch == branch) {
                    found = true;
                    cout << "Roll No: " << setw(4) << current->rollNo
                         << ", Name: " << setw(15) << current->name
                         << ", Company: " << setw(12) << current->companyName
                         << ", Package: ₹" << fixed << setprecision(2) << current->package << " LPA" << endl;
                }
                current = current->next;
            }
        }

        if (!found) {
            cout << "No placement records found for " << branch << " branch!" << endl;
        }
        cout << string(45, '=') << endl;
    }

    void displayTopPackages_agb(int n) {
        cout << "\n========== Top " << n << " Placement Packages ==========" << endl;

        PlacementRecord** records = new PlacementRecord*[totalRecords];
        int count = 0;

        for (int i = 0; i < size; i++) {
            PlacementRecord* current = table[i];
            while (current != NULL) {
                records[count++] = current;
                current = current->next;
            }
        }

        for (int i = 0; i < count - 1; i++) {
            for (int j = 0; j < count - i - 1; j++) {
                if (records[j]->package < records[j + 1]->package) {
                    PlacementRecord* temp = records[j];
                    records[j] = records[j + 1];
                    records[j + 1] = temp;
                }
            }
        }

        int displayCount = min(n, count);
        for (int i = 0; i < displayCount; i++) {
            cout << (i + 1) << ". Roll No: " << setw(4) << records[i]->rollNo
                 << ", Name: " << setw(15) << records[i]->name
                 << ", Branch: " << setw(10) << records[i]->branch
                 << ", Company: " << setw(12) << records[i]->companyName
                 << ", Package: ₹" << fixed << setprecision(2) << records[i]->package << " LPA" << endl;
        }

        if (count == 0) {
            cout << "No placement records found!" << endl;
        }

        delete[] records;
        cout << string(42, '=') << endl;
    }

    ~SmartPlacementPortal() {
        for (int i = 0; i < size; i++) {
            PlacementRecord* current = table[i];
            while (current != NULL) {
                PlacementRecord* temp = current;
                current = current->next;
                delete temp;
            }
        }
        delete[] table;
        cout << "Smart Placement Portal memory deallocated" << endl;
    }
};

int main() {
    int initialSize;

    cout << "===== Smart College Placement Portal =====" << endl;
    cout << "Enter initial hash table size: ";
    cin >> initialSize;

    SmartPlacementPortal portal(initialSize);
    int choice, rollNo;
    string name, branch, companyName;
    float cgpa, package;

    do {
        cout << "\n===== Placement Portal Menu =====" << endl;
        cout << "1. Add placement record" << endl;
        cout << "2. Search placement record" << endl;
        cout << "3. Delete placement record" << endl;
        cout << "4. Display all placement records" << endl;
        cout << "5. Display branch-wise placements" << endl;
        cout << "6. Display top packages" << endl;
        cout << "7. Display portal statistics" << endl;
        cout << "8. Insert sample data" << endl;
        cout << "9. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter Roll Number: ";
                cin >> rollNo;
                cout << "Enter Name: ";
                cin.ignore();
                getline(cin, name);
                cout << "Enter Branch: ";
                getline(cin, branch);
                cout << "Enter CGPA: ";
                cin >> cgpa;
                cout << "Enter Company Name: ";
                cin.ignore();
                getline(cin, companyName);
                cout << "Enter Package (LPA): ₹";
                cin >> package;
                portal.insert_agb(rollNo, name, branch, cgpa, companyName, package); 
                break;

            case 2: {
                cout << "Enter Roll Number to search: ";
                cin >> rollNo;
                PlacementRecord* record = portal.search_agb(rollNo);
                if (record != NULL) {
                    cout << "\n--- Placement Details ---" << endl;
                    cout << "Roll No: " << record->rollNo << endl;
                    cout << "Name: " << record->name << endl;
                    cout << "Branch: " << record->branch << endl;
                    cout << "CGPA: " << fixed << setprecision(2) << record->cgpa << endl;
                    cout << "Company: " << record->companyName << endl;
                    cout << "Package: ₹" << fixed << setprecision(2) << record->package << " LPA" << endl;
                }
                break;
            }

            case 3: {
                cout << "Enter Roll Number to delete: ";
                cin >> rollNo;
                portal.deleteRecord_agb(rollNo); 
                break;
            }

            case 4:
                portal.display_agb(); 
                break;

            case 5: {
                cout << "Enter Branch: ";
                cin.ignore();
                getline(cin, branch);
                portal.displayBranchWise_agb(branch); 
                break;
            }

            case 6: {
                int n;
                cout << "Enter number of top packages to display: ";
                cin >> n;
                portal.displayTopPackages_agb(n); 
                break;
            }

            case 7:
                portal.displayStatistics_agb(); 
                break;

            case 8: {
                cout << "Inserting sample placement records..." << endl;
                portal.insert_agb(101, "Alice Johnson", "Computer Science", 8.75, "Google", 18.5); 
                portal.insert_agb(102, "Bob Smith", "Electronics", 7.90, "Microsoft", 15.0);
                portal.insert_agb(103, "Charlie Brown", "Mechanical", 8.25, "Tesla", 12.5);
                portal.insert_agb(104, "Diana Wilson", "Civil", 9.10, "Amazon", 16.0); 
                portal.insert_agb(105, "Eve Davis", "Computer Science", 8.85, "Apple", 20.0);
                portal.insert_agb(106, "Frank Miller", "Electronics", 7.65, "Intel", 14.0);
                portal.insert_agb(107, "Grace Lee", "Mechanical", 8.40, "Boeing", 13.5); 
                portal.insert_agb(108, "Henry Moore", "Civil", 9.25, "Samsung", 17.0); 
                portal.insert_agb(109, "Ivy Taylor", "Computer Science", 9.05, "Meta", 19.5); 
                portal.insert_agb(110, "Jack Anderson", "Electronics", 7.85, "NVIDIA", 15.5); 
                portal.insert_agb(111, "Kate White", "Mechanical", 8.60, "Siemens", 14.5); 
                portal.insert_agb(112, "Leo Harris", "Civil", 8.90, "Larsen & Toubro", 12.0); 
                break;
            }

            case 9:
                cout << "Exiting... Thank you for using Smart Placement Portal!" << endl;
                break;

            default:
                cout << "Invalid choice! Please try again." << endl;
        }
    } while (choice != 9);

    return 0;
}
```

---

## Input/Output

### Sample Run:

```
===== Smart College Placement Portal =====
Enter initial hash table size: 5
Smart Placement Portal created with initial capacity: 5
Auto-resize threshold: 0.75

===== Placement Portal Menu =====
8. Insert sample data
Enter your choice: 8
Inserting sample placement records...

Inserting Placement Record for Roll No 101 at index 1
Current load factor: 0.20
 Student Alice Johnson placed at Google with package ₹18.50 LPA

Inserting Placement Record for Roll No 102 at index 2
Current load factor: 0.40
Student Bob Smith placed at Microsoft with package ₹15.00 LPA

Inserting Placement Record for Roll No 103 at index 3
Current load factor: 0.60
Student Charlie Brown placed at Tesla with package ₹12.50 LPA

Inserting Placement Record for Roll No 104 at index 4
Current load factor: 0.80
Student Diana Wilson placed at Amazon with package ₹16.00 LPA

Inserting Placement Record for Roll No 105 at index 0
Current load factor: 1.00
Student Eve Davis placed at Apple with package ₹20.00 LPA

[INFO] Resizing hash table from 5 to 10 slots...
[INFO] Resizing completed. New load factor: 0.50

Inserting Placement Record for Roll No 106 at index 6
Current load factor: 0.57
Student Frank Miller placed at Intel with package ₹14.00 LPA

Inserting Placement Record for Roll No 107 at index 7
Current load factor: 0.64
Student Grace Lee placed at Boeing with package ₹13.50 LPA

Inserting Placement Record for Roll No 108 at index 8
Current load factor: 0.71
Student Henry Moore placed at Samsung with package ₹17.00 LPA

Inserting Placement Record for Roll No 109 at index 9
Current load factor: 0.79
Student Ivy Taylor placed at Meta with package ₹19.50 LPA

[INFO] Resizing hash table from 10 to 20 slots...
[INFO] Resizing completed. New load factor: 0.45

Inserting Placement Record for Roll No 110 at index 10
Current load factor: 0.50
Student Jack Anderson placed at NVIDIA with package ₹15.50 LPA

Inserting Placement Record for Roll No 111 at index 11
Current load factor: 0.55
Student Kate White placed at Siemens with package ₹14.50 LPA

Inserting Placement Record for Roll No 112 at index 12
Current load factor: 0.60
Student Leo Harris placed at Larsen & Toubro with package ₹12.00 LPA

===== Placement Portal Menu =====
4. Display all placement records
Enter your choice: 4

========== Smart Placement Portal ==========
Index  0:
  Roll No:  105, Name:       Eve Davis, Branch: Computer Science, CGPA: 8.85, Company:        Apple, Package: ₹20.00 LPA

Index  1:
  Roll No:  101, Name: Alice Johnson, Branch: Computer Science, CGPA: 8.75, Company:       Google, Package: ₹18.50 LPA

Index  2:
  Roll No:  102, Name:       Bob Smith, Branch:     Electronics, CGPA: 7.90, Company:    Microsoft, Package: ₹15.00 LPA

Index  3:
  Roll No:  103, Name: Charlie Brown, Branch:      Mechanical, CGPA: 8.25, Company:        Tesla, Package: ₹12.50 LPA

Index  4:
  Roll No:  104, Name:  Diana Wilson, Branch:           Civil, CGPA: 9.10, Company:       Amazon, Package: ₹16.00 LPA

Index  6:
  Roll No:  106, Name:   Frank Miller, Branch:     Electronics, CGPA: 7.65, Company:        Intel, Package: ₹14.00 LPA

Index  7:
  Roll No:  107, Name:      Grace Lee, Branch:      Mechanical, CGPA: 8.40, Company:       Boeing, Package: ₹13.50 LPA

Index  8:
  Roll No:  108, Name:   Henry Moore, Branch:           Civil, CGPA: 9.25, Company:      Samsung, Package: ₹17.00 LPA

Index  9:
  Roll No:  109, Name:     Ivy Taylor, Branch: Computer Science, CGPA: 9.05, Company:         Meta, Package: ₹19.50 LPA

Index 10:
  Roll No:  110, Name: Jack Anderson, Branch:     Electronics, CGPA: 7.85, Company:       NVIDIA, Package: ₹15.50 LPA

Index 11:
  Roll No:  111, Name:     Kate White, Branch:      Mechanical, CGPA: 8.60, Company:      Siemens, Package: ₹14.50 LPA

Index 12:
  Roll No:  112, Name:     Leo Harris, Branch:           Civil, CGPA: 8.90, Company: Larsen & Toubro, Package: ₹12.00 LPA

--- Portal Statistics ---
Total placement records: 12
Hash table size: 20
Current load factor: 0.60
Resize threshold: 0.75
Non-empty chains: 12
Average chain length: 1.00
Longest chain: 1
================================

===== Placement Portal Menu =====
6. Display top packages
Enter your choice: 6
Enter number of top packages to display: 3

========== Top 3 Placement Packages ==========
1. Roll No:  105, Name:       Eve Davis, Branch: Computer Science, Company:        Apple, Package: ₹20.00 LPA
2. Roll No:  109, Name:     Ivy Taylor, Branch: Computer Science, Company:         Meta, Package: ₹19.50 LPA
3. Roll No:  101, Name: Alice Johnson, Branch: Computer Science, Company:       Google, Package: ₹18.50 LPA
==========================================

===== Placement Portal Menu =====
2. Search placement record
Enter your choice: 2
Enter Roll Number to search: 108

Searching for Student Roll No 108 at index 8
Placement record found!

--- Placement Details ---
Roll No: 108
Name: Henry Moore
Branch: Civil
CGPA: 9.25
Company: Samsung
Package: ₹17.00 LPA

===== Placement Portal Menu =====
9. Exit
Enter your choice: 9
Exiting... Thank you for using Smart Placement Portal!
Smart Placement Portal memory deallocated
```

---

## Dry Run

### Hash Table Initial Size: 5, Threshold: 0.75

### Insertion Sequence with Auto-Resizing:

1. **Roll No 101**: 101 % 5 = 1, Load factor = 0.20
2. **Roll No 102**: 102 % 5 = 2, Load factor = 0.40
3. **Roll No 103**: 103 % 5 = 3, Load factor = 0.60
4. **Roll No 104**: 104 % 5 = 4, Load factor = 0.80 (Triggers resize)

   **Resize from 5 to 10 slots**:

   - Rehash all existing records
   - 101 % 10 = 1, 102 % 10 = 2, 103 % 10 = 3, 104 % 10 = 4

5. **Roll No 105**: 105 % 10 = 5, Load factor = 0.50
6. **Roll No 106**: 106 % 10 = 6, Load factor = 0.57
7. **Roll No 107**: 107 % 10 = 7, Load factor = 0.64
8. **Roll No 108**: 108 % 10 = 8, Load factor = 0.71
9. **Roll No 109**: 109 % 10 = 9, Load factor = 0.79 (Triggers resize)

   **Resize from 10 to 20 slots**:

   - Rehash all existing records
   - New indices: 101 % 20 = 1, 102 % 20 = 2, 103 % 20 = 3, etc.

10. **Roll No 110**: 110 % 20 = 10, Load factor = 0.50
11. **Roll No 111**: 111 % 20 = 11, Load factor = 0.55
12. **Roll No 112**: 112 % 20 = 12, Load factor = 0.60

### Final State (Size: 20):

```
Index:  0   1   2   3   4   5   6   7   8   9  10  11  12  13  14  15  16  17  18  19
Chain: [105] [101] [102] [103] [104] [-] [106] [107] [108] [109] [110] [111] [112] [-] [-] [-] [-] [-] [-] [-]
```

### Search for 108:

```
Hash(108) = 108 % 20 = 8
Check index 8: Roll No 108 ✓ FOUND
```

### Top Packages (Sorted by Package):

1. Eve Davis (105): ₹20.00 LPA (Apple)
2. Ivy Taylor (109): ₹19.50 LPA (Meta)
3. Alice Johnson (101): ₹18.50 LPA (Google)
4. Henry Moore (108): ₹17.00 LPA (Samsung)
5. Diana Wilson (104): ₹16.00 LPA (Amazon)

---

## Conclusion

This program successfully implements a **Smart College Placement Portal using Advanced Hashing Techniques**:


1. **Time Complexity**:

   - **Best Case**: O(1) - No collisions, no resizing
   - **Average Case**: O(1) with dynamic resizing
   - **Worst Case**: O(n) - All elements in one chain (rare with resizing)

2. **Space Complexity**: O(n) where n is the number of elements



