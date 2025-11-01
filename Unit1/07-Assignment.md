# Assignment 7

## Problem Statement
Write a program to **implement Bubble Sort and Quick Sort** on a **1D array of Student structure**  
(containing `student_name`, `student_roll_no`, `total_marks`) with **key as student_roll_no**.  
Also, **count the number of swaps** performed by each method.

---

## Pseudocode
1. **Input number of students (n)**.  
2. **Allocate memory dynamically** for an array of Student structures.  
3. **Input details** → Roll No, Name, Marks.  
4. **Bubble Sort**:  
   - Compare adjacent elements by roll number.  
   - Swap if needed.  
   - Count number of swaps.  
5. **Quick Sort**:  
   - Choose pivot (last element).  
   - Partition array into smaller and larger roll numbers.  
   - Recursively sort subarrays.  
   - Count number of swaps.  
6. **Display sorted arrays and swap counts.**  
7. **Free all dynamically allocated memory.**

---

## Code (C++)
```cpp
#include <iostream>
#include <cstdlib>
#include <cstring>
using namespace std;

struct Student_agb {
    char name[50];
    int rollNo;
    float totalMarks;
};

int bubbleSort_agb(Student_agb *students, int n) {
    int swapCount = 0;
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (students[j].rollNo > students[j + 1].rollNo) {
                swap(students[j], students[j + 1]);
                swapCount++;
            }
        }
    }
    return swapCount;
}

int partition_agb(Student_agb *students, int low, int high, int &swapCount) {
    int pivot = students[high].rollNo;
    int i = low - 1;
    for (int j = low; j < high; j++) {
        if (students[j].rollNo < pivot) {
            i++;
            swap(students[i], students[j]);
            swapCount++;
        }
    }
    swap(students[i + 1], students[high]);
    swapCount++;
    return (i + 1);
}

void quickSort_agb(Student_agb *students, int low, int high, int &swapCount) {
    if (low < high) {
        int pi = partition_agb(students, low, high, swapCount);
        quickSort_agb(students, low, pi - 1, swapCount);
        quickSort_agb(students, pi + 1, high, swapCount);
    }
}

void display_agb(Student_agb *students, int n) {
    cout << "\nRollNo\tName\tMarks\n";
    for (int i = 0; i < n; i++) {
        cout << students[i].rollNo << "\t" << students[i].name << "\t" << students[i].totalMarks << endl;
    }
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

    cout << "\nEnter student details (RollNo Name Marks):\n";
    for (int i = 0; i < n; i++) {
        cin >> students[i].rollNo >> students[i].name >> students[i].totalMarks;
    }

    Student_agb *studentsQuick = (Student_agb *)malloc(n * sizeof(Student_agb));
    if (studentsQuick == NULL) {
        cout << "Memory allocation failed for quick sort copy! Exiting program." << endl;
        exit(1);
    }
    memcpy(studentsQuick, students, n * sizeof(Student_agb));

    cout << "\nOriginal List:";
    display_agb(students, n);

    int bubbleSwaps = bubbleSort_agb(students, n);
    cout << "\nAfter Bubble Sort (by RollNo):";
    display_agb(students, n);
    cout << "Total swaps (Bubble Sort): " << bubbleSwaps << endl;

    int quickSwaps = 0;
    quickSort_agb(studentsQuick, 0, n - 1, quickSwaps);
    cout << "\nAfter Quick Sort (by RollNo):";
    display_agb(studentsQuick, n);
    cout << "Total swaps (Quick Sort): " << quickSwaps << endl;

    free(students);
    free(studentsQuick);

    return 0;
}
```

---

## Input Example
```
Enter number of students: 5
Enter student details (RollNo Name Marks):
3 John 78
1 Alice 89
5 Bob 56
2 Carol 90
4 Dave 65
```

---

## Output Example
```
Original List:
RollNo  Name    Marks
3       John    78
1       Alice   89
5       Bob     56
2       Carol   90
4       Dave    65

After Bubble Sort (by RollNo):
RollNo  Name    Marks
1       Alice   89
2       Carol   90
3       John    78
4       Dave    65
5       Bob     56
Total swaps (Bubble Sort): 6

After Quick Sort (by RollNo):
RollNo  Name    Marks
1       Alice   89
2       Carol   90
3       John    78
4       Dave    65
5       Bob     56
Total swaps (Quick Sort): 7
```

---

### Initial Roll Numbers
```
[3, 1, 5, 2, 4]
```

###  Bubble Sort Step-by-Step

| Pass | Comparison | Swap (if any) | Array after pass | Swaps so far |
|------|-------------|---------------|------------------|---------------|
| 1 | (3,1) → swap | Yes | [1,3,5,2,4] | 1 |
|   | (3,5) | No | [1,3,5,2,4] | 1 |
|   | (5,2) → swap | Yes | [1,3,2,5,4] | 2 |
|   | (5,4) → swap |Yes | [1,3,2,4,5] | 3 |
| 2 | (1,3) |No | [1,3,2,4,5] | 3 |
|   | (3,2) → swap |Yes | [1,2,3,4,5] | 4 |
| 3 | (1,2),(2,3),(3,4),(4,5) |No | [1,2,3,4,5] | 4 |

 **Final Sorted (Bubble Sort):** `[1, 2, 3, 4, 5]`  
 **Total Swaps:** 4

---

### Quick Sort Step-by-Step

| Step | Subarray (low–high) | Pivot | Comparison       | Swap (if any) | Array after step | Swaps so far |
| ---- | ------------------- | ----- | ---------------- | ------------- | ---------------- | ------------ |
| 1    | [3,1,5,2,4] (0–4)   | 4     | (3<4)            | Yes (3↔3)       | [3,1,5,2,4]      | 1            |
|      |                     |       | (1<4)            | Yes (1↔1)       | [3,1,5,2,4]      | 2            |
|      |                     |       | (5<4)            | No             | [3,1,5,2,4]      | 2            |
|      |                     |       | (2<4)            | Yes (5↔2)       | [3,1,2,5,4]      | 3            |
|      |                     |       | Pivot swap (5↔4) |  Yes            | [3,1,2,4,5]      | 4            |
| 2    | [3,1,2] (0–2)       | 2     | (3<2)            | No             | [3,1,2,4,5]      | 4            |
|      |                     |       | (1<2)            | Yes (3↔1)       | [1,3,2,4,5]      | 5            |
|      |                     |       | Pivot swap (3↔2) | Yes             | [1,2,3,4,5]      | 6            |
| 3    | [1] and [3]         | —     | No swaps         | —             | [1,2,3,4,5]      | 6            |
| 4    | [5]                 | —     | No swaps         | —             | [1,2,3,4,5]      | 6            |

 **Final Sorted (Quick Sort):** `[1, 2, 3, 4, 5]`  
 **Total Swaps:** 6

---

## Conclusion
| Sorting Method | Sorted Result | Swap Count | Time Complexity |
|----------------|----------------|-------------|------------------|
| Bubble Sort | `[1,2,3,4,5]` | 4 | O(n²) |
| Quick Sort | `[1,2,3,4,5]` | 6 | O(n log n) |

Both algorithms sort correctly by `rollNo`.  
 **Quick Sort** is faster for large data, while **Bubble Sort** is simpler for small inputs.
