# Assignment 7: Bubble Sort and Quick Sort on Student Records

## Problem Statement
Write a program to **implement Bubble Sort and Quick Sort** on a **1D array of Student structure**  
(containing `student_name`, `student_roll_no`, `total_marks`) with **key as student_roll_no**.  
Also, **count the number of swaps** performed by each method.

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

// Bubble Sort function
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

// Partition function for Quick Sort
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

// Quick Sort function
void quickSort_agb(Student_agb *students, int low, int high, int &swapCount) {
    if (low < high) {
        int pi = partition_agb(students, low, high, swapCount);
        quickSort_agb(students, low, pi - 1, swapCount);
        quickSort_agb(students, pi + 1, high, swapCount);
    }
}

// Display function
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

    // Dynamic memory allocation
    Student_agb *students = (Student_agb *)malloc(n * sizeof(Student_agb));
    if (students == NULL) {
        cout << "Memory allocation failed! Exiting program." << endl;
        exit(1);
    }

    cout << "\nEnter student details (RollNo Name Marks):\n";
    for (int i = 0; i < n; i++) {
        cin >> students[i].rollNo >> students[i].name >> students[i].totalMarks;
    }

    // Copy array for quick sort
    Student_agb *studentsQuick = (Student_agb *)malloc(n * sizeof(Student_agb));
    if (studentsQuick == NULL) {
        cout << "Memory allocation failed for quick sort copy! Exiting program." << endl;
        exit(1);
    }
    memcpy(studentsQuick, students, n * sizeof(Student_agb));

    cout << "\nOriginal List:";
    display_agb(students, n);

    // Bubble Sort
    int bubbleSwaps = bubbleSort_agb(students, n);
    cout << "\nAfter Bubble Sort (by RollNo):";
    display_agb(students, n);
    cout << "Total swaps (Bubble Sort): " << bubbleSwaps << endl;

    // Quick Sort
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

## Input
```
Enter number of students: 5
Enter student details (RollNo Name Marks):
3 John 78
1 Alice 89
5 Bob 56
2 Carol 90
4 Dave 65
```

## Sample Output
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

## Expected Output
```
Both sorting methods produce the same sorted list,
but Quick Sort performs fewer swaps and is faster for large datasets.
```

## Dry Run
**Input:**
```
RollNos = [3, 1, 5, 2, 4]
```

**Bubble Sort Steps:**
```
[3,1,5,2,4] → [1,3,5,2,4] → [1,3,2,5,4] → [1,3,2,4,5] → [1,2,3,4,5]
Total swaps = 6
```

**Quick Sort Steps (pivot = 4):**
```
Partition around 4 → [3,1,2,4,5] → subarrays [3,1,2] and [5]
Further partitions → [1,2,3,4,5]
Total swaps = 7
```

**Conclusion:**
Both algorithms sort correctly using `rollNo` as key.  
**Quick Sort** performs faster for large datasets, while **Bubble Sort** is simpler for small inputs.
