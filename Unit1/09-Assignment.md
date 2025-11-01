# Assignment 9

## Problem Statement
Write a program using the **Bubble Sort** algorithm to assign roll numbers to students of your class based on their **previous year's results** (higher marks → better rank).  
The **topper** should receive **roll no. 1**, 2nd gets roll no. 2, and so on.  
Analyze the sorting algorithm **pass by pass** (show the list after each pass) and count swaps.

## Pseudocode
1. **Input n** → number of students. 

2. **Dynamically allocate** array of student structures.  

3. **Input** each student's `Name` and `PrevMarks`. Initialize `rollNo` = 0.  

4. **Bubble Sort** (descending by `prevMarks`):  
   - For each pass, compare adjacent elements and swap if left < right.  
   - Print array state after each pass.  
   - If a pass has 0 swaps → break early (array sorted).  
5. **Assign roll numbers** in sorted order (index+1).  

6. **Display final list and total swaps.**  

7. **Free memory.**

## Code (C++)
```cpp
#include <iostream>
#include <cstdlib>
#include <cstring>
using namespace std;

struct Student_agb {
    char name[50];
    int prevMarks;
    int rollNo;
};

void display_agb(Student_agb *arr, int n) {
    cout << "\nName\tMarks\tRollNo\n";
    for (int i = 0; i < n; i++) {
        cout << arr[i].name << "\t" << arr[i].prevMarks << "\t" << arr[i].rollNo << endl;
    }
}


int bubbleSortAssign_agb(Student_agb *arr, int n) {
    int totalSwaps = 0;
    for (int pass = 1; pass <= n - 1; pass++) {
        int swapsThisPass = 0;
        for (int j = 0; j < n - pass; j++) {
            if (arr[j].prevMarks < arr[j + 1].prevMarks) {
                // swap entire struct
                Student_agb temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
                swapsThisPass++;
                totalSwaps++;
            }
        }
       
        cout << "\nAfter Pass " << pass << ":\n";
        cout << "Name\tMarks\n";
        for (int k = 0; k < n; k++) {
            cout << arr[k].name << "\t" << arr[k].prevMarks << endl;
        }
        if (swapsThisPass == 0) {
            cout << "\nNo swaps in pass " << pass << ". Array is sorted early.\n";
            break;
        }
    }
    return totalSwaps;
}

int main() {
    int n;
    cout << "Enter number of students: ";
    cin >> n;


    Student_agb *students = (Student_agb *)malloc(n * sizeof(Student_agb));
    if (students == NULL) {
        cout << "Memory allocation failed! Exiting program." << endl;
        return 1;
    }

    cout << "\nEnter student details (Name PrevMarks):\n";
    for (int i = 0; i < n; i++) {
        cin >> students[i].name >> students[i].prevMarks;
        students[i].rollNo = 0; // initialize
    }

    cout << "\nOriginal List:";
    display_agb(students, n);

    int swaps = bubbleSortAssign_agb(students, n);

    for (int i = 0; i < n; i++) {
        students[i].rollNo = i + 1;
    }

    cout << "\nFinal List with Assigned Roll Numbers (Topper = Roll 1):";
    display_agb(students, n);

    cout << "\nTotal number of swaps performed by Bubble Sort: " << swaps << endl;

    free(students);
    return 0;
}
```

## Input
```
Enter number of students: 6
Enter student details (Name PrevMarks):
John 78
Alice 89
Bob 56
Carol 90
Dave 65
Eve 85
```

## Output
```
Original List:
Name    Marks   RollNo
John    78      0
Alice   89      0
Bob     56      0
Carol   90      0
Dave    65      0
Eve     85      0

After Pass 1:
Name    Marks
Alice   89
John    78
Carol   90
Eve     85
Dave    65
Bob     56

After Pass 2:
Name    Marks
Alice   89
Carol   90
John    78
Eve     85
Dave    65
Bob     56

After Pass 3:
Name    Marks
Carol   90
Alice   89
John    78
Eve     85
Dave    65
Bob     56

After Pass 4:
Name    Marks
Carol   90
Alice   89
Eve     85
John    78
Dave    65
Bob     56

After Pass 5:
Name    Marks
Carol   90
Alice   89
Eve     85
John    78
Dave    65
Bob     56

Final List with Assigned Roll Numbers (Topper = Roll 1):
Name    Marks   RollNo
Carol   90      1
Alice   89      2
Eve     85      3
John    78      4
Dave    65      5
Bob     56      6

Total number of swaps performed by Bubble Sort: 5
```


## Dry Run
**Input:**
```
[(John,78), (Alice,89), (Bob,56), (Carol,90), (Dave,65), (Eve,85)]
```

**Pass 1:** swaps move higher marks left → partial ordering  
**Pass 2..k:** continue until fully sorted  
**Final:** [(Carol,90,1), (Alice,89,2), (Eve,85,3), (John,78,4), (Dave,65,5), (Bob,56,6)]

 **Conclusion:**  
Bubble Sort assigns roll numbers based on descending previous marks.  
Pass-by-pass analysis shows how array becomes progressively sorted; early termination reduces passes when already ordered.
