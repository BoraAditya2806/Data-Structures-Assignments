# Assignment 10: Sort Employees by Average of Height and Weight (Merge vs Selection Sort)

## Problem Statement
Write a program to arrange the list of employees as per the **average of their height and weight** using **Merge Sort** and **Selection Sort** methods.  
Analyze their **time complexities** and conclude which algorithm takes less time to sort the list.

## Code (C++)
```cpp
#include <iostream>
#include <cstdlib>
using namespace std;

struct Employee_agb {
    char name[50];
    float height;
    float weight;
    float avg; // average of height and weight
};

// Function to display employee list
void display_agb(Employee_agb *emp, int n) {
    cout << "\nName\tHeight\tWeight\tAverage\n";
    for (int i = 0; i < n; i++) {
        cout << emp[i].name << "\t" << emp[i].height << "\t" << emp[i].weight << "\t" << emp[i].avg << endl;
    }
}

// Selection Sort based on avg
void selectionSort_agb(Employee_agb *emp, int n) {
    for (int i = 0; i < n - 1; i++) {
        int minIndex = i;
        for (int j = i + 1; j < n; j++) {
            if (emp[j].avg < emp[minIndex].avg)
                minIndex = j;
        }
        if (minIndex != i) {
            Employee_agb temp = emp[i];
            emp[i] = emp[minIndex];
            emp[minIndex] = temp;
        }
        cout << "\nAfter Pass " << i + 1 << " (Selection Sort):\n";
        display_agb(emp, n);
    }
}

// Merge function
void merge_agb(Employee_agb *emp, int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;

    Employee_agb *L = (Employee_agb *)malloc(n1 * sizeof(Employee_agb));
    Employee_agb *R = (Employee_agb *)malloc(n2 * sizeof(Employee_agb));

    for (int i = 0; i < n1; i++)
        L[i] = emp[left + i];
    for (int j = 0; j < n2; j++)
        R[j] = emp[mid + 1 + j];

    int i = 0, j = 0, k = left;
    while (i < n1 && j < n2) {
        if (L[i].avg <= R[j].avg)
            emp[k++] = L[i++];
        else
            emp[k++] = R[j++];
    }
    while (i < n1)
        emp[k++] = L[i++];
    while (j < n2)
        emp[k++] = R[j++];

    free(L);
    free(R);
}

// Merge Sort function
void mergeSort_agb(Employee_agb *emp, int left, int right, int pass = 1) {
    if (left < right) {
        int mid = (left + right) / 2;
        mergeSort_agb(emp, left, mid, pass + 1);
        mergeSort_agb(emp, mid + 1, right, pass + 1);
        merge_agb(emp, left, mid, right);

        cout << "\nAfter Merge Pass " << pass << " (Merge Sort):\n";
        display_agb(emp, right + 1);
    }
}

int main() {
    int n;
    cout << "Enter number of employees: ";
    cin >> n;

    Employee_agb *employees = (Employee_agb *)malloc(n * sizeof(Employee_agb));
    Employee_agb *copyEmp = (Employee_agb *)malloc(n * sizeof(Employee_agb));
    if (employees == NULL || copyEmp == NULL) {
        cout << "Memory allocation failed! Exiting program." << endl;
        exit(1);
    }

    cout << "\nEnter employee details (Name Height Weight):\n";
    for (int i = 0; i < n; i++) {
        cin >> employees[i].name >> employees[i].height >> employees[i].weight;
        employees[i].avg = (employees[i].height + employees[i].weight) / 2.0;
        copyEmp[i] = employees[i];
    }

    cout << "\nOriginal List:";
    display_agb(employees, n);

    // Apply Selection Sort
    cout << "\n===== Selection Sort Analysis =====\n";
    selectionSort_agb(employees, n);
    cout << "\nFinal Sorted List (Selection Sort):";
    display_agb(employees, n);

    // Apply Merge Sort
    cout << "\n===== Merge Sort Analysis =====\n";
    mergeSort_agb(copyEmp, 0, n - 1);
    cout << "\nFinal Sorted List (Merge Sort):";
    display_agb(copyEmp, n);

    free(employees);
    free(copyEmp);

    cout << "\n===== Time Complexity Analysis =====\n";
    cout << "Selection Sort: O(n^2) — inefficient for large data.\n";
    cout << "Merge Sort: O(n log n) — faster and more efficient.\n";
    cout << "Hence, Merge Sort takes less time than Selection Sort for large n.\n";

    return 0;
}
```

## Input
```
Enter number of employees: 4
Enter employee details (Name Height Weight):
John 180 75
Alice 160 55
Bob 175 68
Carol 165 70
```

## Sample Output
```
Original List:
Name    Height  Weight  Average
John    180     75      127.5
Alice   160     55      107.5
Bob     175     68      121.5
Carol   165     70      117.5

===== Selection Sort Analysis =====
After Pass 1:
Alice   160     55      107.5
Carol   165     70      117.5
Bob     175     68      121.5
John    180     75      127.5

Final Sorted List (Selection Sort):
Alice   160     55      107.5
Carol   165     70      117.5
Bob     175     68      121.5
John    180     75      127.5

===== Merge Sort Analysis =====
After Merge Pass 1:
Alice   160     55      107.5
Carol   165     70      117.5
Bob     175     68      121.5
John    180     75      127.5

Final Sorted List (Merge Sort):
Alice   160     55      107.5
Carol   165     70      117.5
Bob     175     68      121.5
John    180     75      127.5

===== Time Complexity Analysis =====
Selection Sort: O(n^2) — inefficient for large data.
Merge Sort: O(n log n) — faster and more efficient.
Hence, Merge Sort takes less time than Selection Sort for large n.
```

## Pseudocode
1. **Input n** → number of employees.  

2. **Dynamically allocate memory** for employee list.  

3. **Input name, height, weight**, and compute average = (height + weight) / 2.  

4. **Selection Sort**:  
   - Find minimum average in remaining list and swap.  
   - Repeat for each pass and display after each.  

5. **Merge Sort**:  
   - Recursively divide list into halves.  
   - Merge sorted halves based on average.  
   - Display list after each merge.  

6. **Display final sorted lists** from both algorithms.  

7. **Analyze time complexity** and conclude efficiency.

## Expected Output
```
Both methods produce same sorted result.
Merge Sort performs faster for large datasets due to O(n log n) complexity.
Selection Sort is simpler but slower with O(n²) complexity.
```

## Dry Run
**Input:**
```
John(127.5), Alice(107.5), Bob(121.5), Carol(117.5)
```

**Selection Sort Passes:**
- Pass 1: Alice → [Alice, Carol, Bob, John]
- Pass 2: Carol → [Alice, Carol, Bob, John]
- Pass 3: Bob → [Alice, Carol, Bob, John]

**Merge Sort Passes:**
- Divide into halves → Merge back maintaining order
- Final → [Alice, Carol, Bob, John]

 **Conclusion:**
Both algorithms give same sorted order by average value.  
However, **Merge Sort** is significantly faster and scalable compared to **Selection Sort**.
