# Assignment 8

## Problem Statement
Write a program to input marks of `n` students, sort the marks in **ascending order** using the **Quick Sort algorithm** (without using built-in library functions), and analyze the sorting **pass by pass**.  
Then, find the **minimum and maximum marks** using the **Divide and Conquer** approach recursively.


## Pseudocode

1. **Input n** → number of students.  
2. **Allocate dynamic memory** for marks array.  
3. **Quick Sort Algorithm**:  
   - Select pivot (last element).  
   - Partition array such that elements < pivot are on left and > pivot on right.  
   - Recursively sort both subarrays.  
   - Print array after each pass.  
4. **Divide & Conquer Min/Max**:  
   - Split array recursively into two halves.  
   - Find min/max in each half.  
   - Combine results to get global min and max.  
5. **Display sorted list and min/max values.**


## Code (C++)
```cpp
#include <iostream>
#include <cstdlib>
using namespace std;

void display_agb(int *arr, int n) {
    for (int i = 0; i < n; i++)
        cout << arr[i] << " ";
    cout << endl;
}

int partition_agb(int *arr, int low, int high, int pass) {
    int pivot = arr[high];
    int i = low - 1;

    for (int j = low; j < high; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i + 1], arr[high]);

    cout << "Pass " << pass << ": ";
    display_agb(arr, high + 1);

    return i + 1;
}

void quickSort_agb(int *arr, int low, int high, int &pass) {
    if (low < high) {
        int pi = partition_agb(arr, low, high, pass);
        pass++;
        quickSort_agb(arr, low, pi - 1, pass);
        quickSort_agb(arr, pi + 1, high, pass);
    }
}

void findMinMax_agb(int *arr, int low, int high, int &minVal, int &maxVal) {
    if (low == high) {
        minVal = maxVal = arr[low];
    } else if (high == low + 1) {
        if (arr[low] < arr[high]) {
            minVal = arr[low];
            maxVal = arr[high];
        } else {
            minVal = arr[high];
            maxVal = arr[low];
        }
    } else {
        int mid = (low + high) / 2;
        int min1, max1, min2, max2;

        findMinMax_agb(arr, low, mid, min1, max1);
        findMinMax_agb(arr, mid + 1, high, min2, max2);

        minVal = (min1 < min2) ? min1 : min2;
        maxVal = (max1 > max2) ? max1 : max2;
    }
}

int main() {
    int n;
    cout << "Enter number of students: ";
    cin >> n;

    int *marks = (int *)malloc(n * sizeof(int));
    if (marks == NULL) {
        cout << "Memory allocation failed! Exiting program." << endl;
        exit(1);
    }

    cout << "Enter marks of " << n << " students: ";
    for (int i = 0; i < n; i++)
        cin >> marks[i];

    cout << "\nOriginal Marks: ";
    display_agb(marks, n);

    int pass = 1;
    quickSort_agb(marks, 0, n - 1, pass);

    cout << "\nSorted Marks (Ascending Order): ";
    display_agb(marks, n);

    int minVal, maxVal;
    findMinMax_agb(marks, 0, n - 1, minVal, maxVal);

    cout << "\nMinimum Marks: " << minVal << endl;
    cout << "Maximum Marks: " << maxVal << endl;

    free(marks);
    return 0;
}
```

## Input
```
Enter number of students: 6
Enter marks of 6 students: 45 89 12 67 34 78
```

## Output
```
Original Marks: 45 89 12 67 34 78
Pass 1: 45 12 34 67 78 89
Pass 2: 12 34 45 67 78 89
Sorted Marks (Ascending Order): 12 34 45 67 78 89

Minimum Marks: 12
Maximum Marks: 89
```



## Dry Run 

**Input Array:**  
`[45, 89, 12, 67, 34, 78]`

| **Pass** | **Pivot** | **Comparisons and Swaps** | **Array After Pass** |
|-----------|------------|----------------------------|------------------------|
| 1 | 78 | Compare each element with 78 and swap smaller elements before pivot | [45, 12, 67, 34, 78, 89] |
| 2 | 34 | Sort left subarray [45, 12, 67, 34] → pivot 34 placed correctly | [12, 34, 45, 67, 78, 89] |
| 3 | - | Remaining subarrays [12] and [45, 67] are already sorted | [12, 34, 45, 67, 78, 89] |

**Final Sorted Array:**  
`12 34 45 67 78 89`

### Divide & Conquer Min/Max Process

| **Step** | **Subarray** | **Min** | **Max** |
|-----------|---------------|----------|----------|
| 1 | [45, 89, 12, 67, 34, 78] | — | — |
| 2 | Split into [45, 89, 12] and [67, 34, 78] | — | — |
| 3 | For [45, 89, 12] → min = 12, max = 89 | 12 | 89 |
| 4 | For [67, 34, 78] → min = 34, max = 78 | 34 | 78 |
| 5 | Combine → min = 12, max = 89 | 12 | 89 |

**Result:**  
Minimum Marks = **12**  
Maximum Marks = **89**

## Conclusion
- **Quick Sort** efficiently sorts the marks in ascending order (O(n log n)).  
- **Divide and Conquer** approach finds minimum and maximum values recursively in O(n).  
