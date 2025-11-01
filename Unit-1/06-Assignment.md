# Assignment 6

## Problem Statement
In the Computer Engineering Department of VIT, there are **S.Y., T.Y., and B.Tech. students** on the ground for a function.  
We need to identify a **student of S.Y. Division (X)** whose **name is "XYZ"** and **roll number is 17**.  
An appropriate **searching method** should be applied to identify the required student.


## Pseudocode

1. **Input n** → total number of students.  

2. **Allocate memory dynamically** for storing student records. 

3. **Input details** → Roll No, Name, Division, Year.  

4. **Set search criteria**: Roll No = 17, Name = “XYZ”, Division = “X”, Year = “SY”. 

5. **Apply Linear Search**:  
   - Compare each record’s roll no, name, division, and year.  
   - If all match → student found.  
   - Else → continue searching.  
6. **Display result** accordingly.  

7. **Free allocated memory.**


## Code (C++)
```cpp
#include <iostream>
using namespace std;

struct Student_agb {
    int rollNo;
    char name[50];
    char division[10];
    char year[10];
};


int linearSearch_agb(Student_agb *students, int n, int rollNo, const char *name, const char *division, const char *year) {
    for (int i = 0; i < n; i++) {
        if (students[i].rollNo == rollNo &&
            strcmp(students[i].name, name) == 0 &&
            strcmp(students[i].division, division) == 0 &&
            strcmp(students[i].year, year) == 0) {
            return i; 
        }
    }
    return -1; 
}

int main() {
    int n;
    cout << "Enter number of students: ";
    cin >> n;


    Student_agb *students = (Student_agb *)malloc(n * sizeof(Student_agb));
    if (students == NULL) {
        cout << "Memory allocation failed! Exiting program." << endl;
        exit(1);
    }

    
    cout << "\nEnter student details (RollNo Name Division Year):\n";
    for (int i = 0; i < n; i++) {
        cin >> students[i].rollNo >> students[i].name >> students[i].division >> students[i].year;
    }

    int searchRoll = 17;
    const char searchName[] = "XYZ";
    const char searchDiv[] = "X";
    const char searchYear[] = "SY";

    int pos = linearSearch_agb(students, n, searchRoll, searchName, searchDiv, searchYear);

    if (pos != -1) {
        cout << "\nStudent Found!\n";
        cout << "Roll No: " << students[pos].rollNo << endl;
        cout << "Name: " << students[pos].name << endl;
        cout << "Division: " << students[pos].division << endl;
        cout << "Year: " << students[pos].year << endl;
    } else {
        cout << "\nStudent Not Found in Records.\n";
    }

    free(students);
    return 0;
}
```

## Input
```
Enter number of students: 5
Enter student details (RollNo Name Division Year):
10 ABC X SY
17 XYZ X SY
25 DEF Y TY
30 PQR Z BTECH
40 LMN X TY
```

## Output
```
Student Found!
Roll No: 17
Name: XYZ
Division: X
Year: SY
```

## Dry Run
**Input:**
```
n = 5
Students:
1) (10, ABC, X, SY)
2) (17, XYZ, X, SY)
3) (25, DEF, Y, TY)
4) (30, PQR, Z, BTECH)
5) (40, LMN, X, TY)
Search Criteria: Roll=17, Name=XYZ, Division=X, Year=SY
```

**Step 1:** Compare with 1st → no match  
**Step 2:** Compare with 2nd → match found  
**Output:** Student details displayed

 **Conclusion:**
The required student was successfully identified using **Linear Search**.  
This method is simple and effective for small datasets.
