# Assignment 8 

## Problem Statement

Write a program to efficiently search a particular employee record by using Tree data structure. Also sort the data on emp­id in ascending order.

---

## Pseudocode

```
CLASS Employee:
    empID: integer
    name: string
    department: string
    salary: float
    left: pointer to Employee
    right: pointer to Employee

CLASS EmployeeBST:
    root: pointer to Employee

    FUNCTION insertEmployee(empID, name, dept, salary):
        root = insertRecursive(root, empID, name, dept, salary)

    FUNCTION insertRecursive(node, empID, name, dept, salary):
        IF node == NULL:
            CREATE new Employee
            RETURN new Employee

        IF empID < node.empID:
            node.left = insertRecursive(node.left, empID, name, dept, salary)
        ELSE IF empID > node.empID:
            node.right = insertRecursive(node.right, empID, name, dept, salary)
        ELSE:
            PRINT "Duplicate employee ID"

        RETURN node

    FUNCTION searchEmployee(empID):
        RETURN searchRecursive(root, empID)

    FUNCTION searchRecursive(node, empID):
        IF node == NULL OR node.empID == empID:
            RETURN node

        IF empID < node.empID:
            RETURN searchRecursive(node.left, empID)
        RETURN searchRecursive(node.right, empID)

    FUNCTION displayInorder(node):
        IF node != NULL:
            displayInorder(node.left)
            PRINT node.empID, node.name, node.department, node.salary
            displayInorder(node.right)

    FUNCTION sortEmployees():
        displayInorder(root)

MAIN:
    CREATE EmployeeBST
    INSERT employees
    SEARCH employee by ID
    SORT and DISPLAY all employees
```

---

## C++ Code

```cpp
#include <iostream>
#include <string>
#include <iomanip>
using namespace std;

class Employee {
public:
    int empID;
    string name;
    string department;
    float salary;
    Employee* left;
    Employee* right;

    Employee_agb(int id, string n, string dept, float sal) {
        empID = id;
        name = n;
        department = dept;
        salary = sal;
        left = NULL;
        right = NULL;
    }
};

class EmployeeBST {
private:
    Employee* root_agb;

    Employee* insertRecursive_agb(Employee* node_agb, int empID_agb, string name_agb, string dept_agb, float salary_agb) {
        if (node_agb == NULL) {
            return new Employee_agb(empID_agb, name_agb, dept_agb, salary_agb);
        }

        if (empID_agb < node_agb->empID) {
            node_agb->left = insertRecursive_agb(node_agb->left, empID_agb, name_agb, dept_agb, salary_agb);
        } else if (empID_agb > node_agb->empID) {
            node_agb->right = insertRecursive_agb(node_agb->right, empID_agb, name_agb, dept_agb, salary_agb);
        } else {
            cout << " Duplicate Employee ID " << empID_agb << "! Record not inserted." << endl;
        }

        return node_agb;
    }

    Employee* searchRecursive_agb(Employee* node_agb, int empID_agb) {
        if (node_agb == NULL || node_agb->empID == empID_agb) {
            return node_agb;
        }

        if (empID_agb < node_agb->empID) {
            return searchRecursive_agb(node_agb->left, empID_agb);
        }
        return searchRecursive_agb(node_agb->right, empID_agb);
    }

    void inorderHelper_agb(Employee* node_agb) {
        if (node_agb != NULL) {
            inorderHelper_agb(node_agb->left);
            cout << setw(8) << node_agb->empID
                 << setw(20) << node_agb->name
                 << setw(15) << node_agb->department
                 << setw(12) << fixed << setprecision(2)
                 << node_agb->salary << endl;
            inorderHelper_agb(node_agb->right);
        }
    }

    void preorderHelper_agb(Employee* node_agb) {
        if (node_agb != NULL) {
            cout << setw(8) << node_agb->empID
                 << setw(20) << node_agb->name
                 << setw(15) << node_agb->department
                 << setw(12) << fixed << setprecision(2)
                 << node_agb->salary << endl;
            preorderHelper_agb(node_agb->left);
            preorderHelper_agb(node_agb->right);
        }
    }

    int countEmployees_agb(Employee* node_agb) {
        if (node_agb == NULL) {
            return 0;
        }
        return 1 + countEmployees_agb(node_agb->left) + countEmployees_agb(node_agb->right);
    }

    Employee* findMin_agb(Employee* node_agb) {
        while (node_agb->left != NULL) {
            node_agb = node_agb->left;
        }
        return node_agb;
    }

    Employee* findMax_agb(Employee* node_agb) {
        while (node_agb->right != NULL) {
            node_agb = node_agb->right;
        }
        return node_agb;
    }

public:
    EmployeeBST_agb() {
        root_agb = NULL;
    }

    void insertEmployee_agb(int empID_agb, string name_agb, string dept_agb, float salary_agb) {
        root_agb = insertRecursive_agb(root_agb, empID_agb, name_agb, dept_agb, salary_agb);
        Employee* emp_agb = searchRecursive_agb(root_agb, empID_agb);
        if (emp_agb != NULL && emp_agb->empID == empID_agb) { 
            cout << "Employee '" << name_agb << "' (ID: " << empID_agb << ") added successfully!" << endl;
        }
    }

    void searchEmployee_agb(int empID_agb) {
        Employee* emp_agb = searchRecursive_agb(root_agb, empID_agb);
        if (emp_agb != NULL) {
            cout << "\n===== Employee Record Found =====" << endl;
            cout << "Employee ID: " << emp_agb->empID << endl;
            cout << "Name: " << emp_agb->name << endl;
            cout << "Department: " << emp_agb->department << endl;
            cout << "Salary: $" << fixed << setprecision(2) << emp_agb->salary << endl;
        } else {
            cout << "✗ Employee with ID " << empID_agb << " not found!" << endl;
        }
    }

    void sortAndDisplay_agb() {
        if (root_agb == NULL) {
            cout << "\nNo employee records found!" << endl;
            return;
        }

        cout << "\n========== Employee Records (Sorted by ID) ==========" << endl;
        cout << setw(8) << "Emp ID"
             << setw(20) << "Name"
             << setw(15) << "Department"
             << setw(12) << "Salary" << endl;
        cout << string(55, '-') << endl;

        inorderHelper_agb(root_agb);
        cout << string(55, '=') << endl;
        cout << "Total employees: " << countEmployees_agb(root_agb) << endl;
    }

    void displayPreorder_agb() {
        if (root_agb == NULL) {
            cout << "\nNo employee records found!" << endl;
            return;
        }

        cout << "\n========== Employee Records (Preorder) ==========" << endl;
        cout << setw(8) << "Emp ID"
             << setw(20) << "Name"
             << setw(15) << "Department"
             << setw(12) << "Salary" << endl;
        cout << string(55, '-') << endl;

        preorderHelper_agb(root_agb);
        cout << string(55, '=') << endl;
    }

    void displayStats_agb() {
        if (root_agb == NULL) {
            cout << "\nNo employee records found!" << endl;
            return;
        }

        cout << "\n===== Employee Database Statistics =====" << endl;
        cout << "Total employees: " << countEmployees_agb(root_agb) << endl;

        Employee* minEmp_agb = findMin_agb(root_agb);
        Employee* maxEmp_agb = findMax_agb(root_agb);

        if (minEmp_agb != NULL) {
            cout << "Lowest Employee ID: " << minEmp_agb->empID
                 << " (" << minEmp_agb->name << ")" << endl;
        }

        if (maxEmp_agb != NULL) {
            cout << "Highest Employee ID: " << maxEmp_agb->empID
                 << " (" << maxEmp_agb->name << ")" << endl;
        }
    }

    void displayByDepartment_agb(string dept_agb) {
        cout << "\n========== Employees in " << dept_agb << " Department ==========" << endl;
        cout << setw(8) << "Emp ID"
             << setw(20) << "Name"
             << setw(15) << "Department"
             << setw(12) << "Salary" << endl;
        cout << string(55, '-') << endl;

        displayByDepartmentHelper_agb(root_agb, dept_agb);
        cout << string(55, '=') << endl;
    }

    void displayByDepartmentHelper_agb(Employee* node_agb, string dept_agb) {
        if (node_agb != NULL) {
            displayByDepartmentHelper_agb(node_agb->left, dept_agb);
            if (node_agb->department == dept_agb) {
                cout << setw(8) << node_agb->empID
                     << setw(20) << node_agb->name
                     << setw(15) << node_agb->department
                     << setw(12) << fixed << setprecision(2)
                     << node_agb->salary << endl;
            }
            displayByDepartmentHelper_agb(node_agb->right, dept_agb);
        }
    }
};

int main_agb() {
    EmployeeBST empDB_agb;
    int choice_agb;

    do {
        cout << "\n===== Employee Record Management System =====" << endl;
        cout << "1. Add employee record" << endl;
        cout << "2. Search employee by ID" << endl;
        cout << "3. Display all employees (sorted by ID)" << endl;
        cout << "4. Display all employees (preorder)" << endl;
        cout << "5. Display statistics" << endl;
        cout << "6. Display employees by department" << endl;
        cout << "7. Add sample employees" << endl;
        cout << "8. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice_agb;
        cin.ignore();

        switch (choice_agb) {
            case 1: {
                int empID_agb;
                string name_agb, dept_agb;
                float salary_agb;

                cout << "\n--- Add New Employee ---" << endl;
                cout << "Enter Employee ID: ";
                cin >> empID_agb;
                cin.ignore();
                cout << "Enter Name: ";
                getline(cin, name_agb);
                cout << "Enter Department: ";
                getline(cin, dept_agb);
                cout << "Enter Salary: $";
                cin >> salary_agb;

                empDB_agb.insertEmployee_agb(empID_agb, name_agb, dept_agb, salary_agb);
                break;
            }

            case 2: {
                int empID_agb;
                cout << "\nEnter Employee ID to search: ";
                cin >> empID_agb;
                empDB_agb.searchEmployee_agb(empID_agb);
                break;
            }

            case 3:
                empDB_agb.sortAndDisplay_agb();
                break;

            case 4:
                empDB_agb.displayPreorder_agb();
                break;

            case 5:
                empDB_agb.displayStats_agb();
                break;

            case 6: {
                string dept_agb;
                cout << "\nEnter department name: ";
                getline(cin, dept_agb);
                empDB_agb.displayByDepartment_agb(dept_agb);
                break;
            }

            case 7: {
                int ids_agb[] = {103, 101, 105, 102, 104, 107, 106, 108, 109, 110};
                string names_agb[] = {"Alice Johnson", "Bob Smith", "Charlie Brown", "Diana Prince",
                                 "Edward Norton", "Fiona Gallagher", "George Wilson",
                                 "Helen Carter", "Ian Malcolm", "Jane Doe"};
                string depts_agb[] = {"IT", "HR", "Finance", "IT", "Marketing",
                                 "Finance", "HR", "IT", "Marketing", "Finance"};
                float salaries_agb[] = {75000.50, 65000.00, 70000.75, 80000.00, 60000.25,
                                    72000.00, 68000.50, 85000.00, 62000.75, 71000.00};

                cout << "\nAdding sample employees:" << endl;
                for (int i_agb = 0; i_agb < 10; i_agb++) {
                    empDB_agb.insertEmployee_agb(ids_agb[i_agb], names_agb[i_agb], depts_agb[i_agb], salaries_agb[i_agb]);
                }
                break;
            }

            case 8:
                cout << "Exiting... Thank you!" << endl;
                break;

            default:
                cout << "Invalid choice!" << endl;
        }
    } while (choice_agb != 8);

    return 0;
}
```

---

## Input/Output

### Sample Run:

```
===== Employee Record Management System =====
Enter your choice: 7

Adding sample employees:
Employee 'Alice Johnson' (ID: 103) added successfully!
Employee 'Bob Smith' (ID: 101) added successfully!
Employee 'Charlie Brown' (ID: 105) added successfully!
Employee 'Diana Prince' (ID: 102) added successfully!
Employee 'Edward Norton' (ID: 104) added successfully!
Employee 'Fiona Gallagher' (ID: 107) added successfully!
Employee 'George Wilson' (ID: 106) added successfully!
Employee 'Helen Carter' (ID: 108) added successfully!
Employee 'Ian Malcolm' (ID: 109) added successfully!
Employee 'Jane Doe' (ID: 110) added successfully!

Enter your choice: 3

========== Employee Records (Sorted by ID) ==========
  Emp ID                Name    Department      Salary
-------------------------------------------------------
     101           Bob Smith            HR    65000.00
     102        Diana Prince            IT    80000.00
     103       Alice Johnson            IT    75000.50
     104      Edward Norton     Marketing    60000.25
     105      Charlie Brown       Finance    70000.75
     106      George Wilson            HR    68000.50
     107   Fiona Gallagher       Finance    72000.00
     108       Helen Carter            IT    85000.00
     109        Ian Malcolm     Marketing    62000.75
     110            Jane Doe       Finance    71000.00
=======================================================
Total employees: 10

Enter your choice: 2

Enter Employee ID to search: 105

===== Employee Record Found =====
Employee ID: 105
Name: Charlie Brown
Department: Finance
Salary: $70000.75

Enter your choice: 2

Enter Employee ID to search: 200
Employee with ID 200 not found!

Enter your choice: 6

Enter department name: IT

========== Employees in IT Department ==========
  Emp ID                Name    Department      Salary
-------------------------------------------------------
     102        Diana Prince            IT    80000.00
     103       Alice Johnson            IT    75000.50
     108       Helen Carter            IT    85000.00
=======================================================

Enter your choice: 5

===== Employee Database Statistics =====
Total employees: 10
Lowest Employee ID: 101 (Bob Smith)
Highest Employee ID: 110 (Jane Doe)

Enter your choice: 8
Exiting... Thank you!
```

---

## Dry Run

### BST Construction with Sample Data:

```
Employee IDs to insert in order: 103, 101, 105, 102, 104

Step 1: Insert 103 (Root)
       103

Step 2: Insert 101 (101 < 103 → Left)
       103
      /
    101

Step 3: Insert 105 (105 > 103 → Right)
       103
      /  \
   101    105

Step 4: Insert 102 (102 > 101 → Right, 102 < 103 → Left)
       103
      /  \
   101    105
     \
     102

Step 5: Insert 104 (104 > 103 → Right, 104 < 105 → Left)
       103
      /  \
   101    105
     \    /
     102 104

Final BST structure:
         103
       /    \
     101     105
       \     /  \
      102  104   ...
```

### Search Employee (ID = 104)

```
Step 1: Start at root (103)
  104 > 103 → go right

Step 2: At node 105
  104 < 105 → go left

Step 3: At node 104
  104 == 104 → FOUND

Return pointer to employee record with ID 104
```

### Sort and Display (Inorder Traversal)

```
inorder(103):
  inorder(101):
    inorder(NULL) → return
    print 101 (Bob Smith)
    inorder(102):
      inorder(NULL) → return
      print 102 (Diana Prince)
      inorder(NULL) → return
  print 103 (Alice Johnson)
  inorder(105):
    inorder(104):
      inorder(NULL) → return
      print 104 (Edward Norton)
      inorder(NULL) → return
    print 105 (Charlie Brown)
    inorder(...):
      ...

Output (sorted by empID):
101 Bob Smith HR 65000.00
102 Diana Prince IT 80000.00
103 Alice Johnson IT 75000.50
104 Edward Norton Marketing 60000.25
105 Charlie Brown Finance 70000.75
...
```

### Display by Department (IT Department)

```
Traverse entire tree in inorder:
For each employee:
  If department == "IT":
    Print employee details

Employees in IT department:
- 102 Diana Prince: $80000.00
- 103 Alice Johnson: $75000.50
- 108 Helen Carter: $85000.00
```

---

## Conclusion

This program implements an **Employee Record Management System using BST** with the following achievements:

1. **BST-Based Employee Database**:

   - Employees stored and organized by empID
   - Efficient O(log n) search operations
   - Automatic sorting by empID (ascending)
   - No duplicate employee IDs allowed

2. **Core Operations**:

   - **Insert**: O(log n) average case
   - **Search**: O(log n) average case
   - **Sort**: O(n) using inorder traversal
   - **Display**: Multiple viewing options

3. **Employee Record Structure**:

   - Unique Employee ID (primary key)
   - Name
   - Department
   - Salary
   - Left and right pointers for BST

4. **Key Features**:

   - Fast employee search by ID
   - Automatic sorting during display
   - Department-based filtering
   - Database statistics
   - Sample data creation
   - Error handling for duplicates

5. **Time Complexity**:

   - Insert: O(log n) average, O(n) worst case
   - Search: O(log n) average, O(n) worst case
   - Sort/Display: O(n)
   - Department filter: O(n)

6. **Space Complexity**: O(n) for storing employee records

**BST Advantages for Employee Database**:

- **Fast Search**: Logarithmic time complexity
- **Automatic Sorting**: Inorder traversal gives sorted output
- **Efficient Range Queries**: Easy to find employees in ID ranges
- **Memory Efficient**: No wasted space compared to arrays
- **Dynamic Size**: Grows and shrinks as needed

**Applications**:

- HR management systems
- Employee directories
- Payroll systems
- Performance tracking databases
- Corporate phone directories

**Real-world Benefits**:

- Quick employee lookup by ID
- Organized data presentation
- Efficient data management
- Scalable for large organizations
- Easy to extend with additional fields

**Future Enhancements**:

- Multiple search criteria (name, department, salary range)
- Update employee records
- Delete employee records
- Export to file formats
- Advanced filtering and reporting
