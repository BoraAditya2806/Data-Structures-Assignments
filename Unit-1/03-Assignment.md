# Assignment 3: Matrix Multiplication – Row-Major vs Column-Major 

## Pseudocode
1. **Input n**
   - Read order of matrix.

2. **Dynamically allocate memory**
   - Allocate 2D arrays A, B, C using `malloc`.
   - If any allocation fails, print error and exit.

3. **Input matrices**
   - Take user input for A and B.

4. **Row-Major Multiplication**
   - Use `C[i][j] += A[i][k] * B[k][j]`.

5. **Column-Major Multiplication**
   - Use `C[i][j] += A[i][k] * B[j][k]`.

6. **Display results**
   - Print both matrices.

7. **Free memory**
   - Use `free()` to deallocate A, B, and C.
     


## Code (C++)
```cpp
#include <iostream>
using namespace std;


void multiplyRowMajor_agb(int **A, int **B, int **C, int n) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            C[i][j] = 0;
            for (int k = 0; k < n; k++) {
                C[i][j] += A[i][k] * B[k][j]; 
            }
        }
    }
}

void multiplyColumnMajor_agb(int **A, int **B, int **C, int n) {
    for (int j = 0; j < n; j++) {
        for (int i = 0; i < n; i++) {
            C[i][j] = 0;
            for (int k = 0; k < n; k++) {
                C[i][j] += A[i][k] * B[j][k]; 
            }
        }
    }
}

int main() {
    int n;
    cout << "Enter order of matrix (n x n): ";
    cin >> n;

    
    int **A = (int **)malloc(n * sizeof(int *));
    int **B = (int **)malloc(n * sizeof(int *));
    int **C = (int **)malloc(n * sizeof(int *));
    
    if (A == NULL || B == NULL || C == NULL) {
        cout << "Memory allocation failed! Exiting program." << endl;
        exit(1);
    }

    for (int i = 0; i < n; i++) {
        A[i] = (int *)malloc(n * sizeof(int));
        B[i] = (int *)malloc(n * sizeof(int));
        C[i] = (int *)malloc(n * sizeof(int));
        if (A[i] == NULL || B[i] == NULL || C[i] == NULL) {
            cout << "Memory allocation failed for row " << i << ". Exiting program." << endl;
            exit(1);
        }
    }


    cout << "Enter elements of first matrix (A):" << endl;
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            cin >> A[i][j];

    cout << "Enter elements of second matrix (B):" << endl;
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            cin >> B[i][j];

   
    multiplyRowMajor_agb(A, B, C, n);
    cout << "\nResult using Row-Major access:\n";
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++)
            cout << C[i][j] << " ";
        cout << endl;
    }

    multiplyColumnMajor_agb(A, B, C, n);
    cout << "\nResult using Column-Major access:\n";
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++)
            cout << C[i][j] << " ";
        cout << endl;
    }

    for (int i = 0; i < n; i++) {
        free(A[i]);
        free(B[i]);
        free(C[i]);
    }
    free(A);
    free(B);
    free(C);

    return 0;
}
```

## Input
```
Enter order of matrix (n x n): 2
Enter elements of first matrix (A):
1 2
3 4
Enter elements of second matrix (B):
5 6
7 8
```

## Output
```
Result using Row-Major access:
19 22
43 50

Result using Column-Major access:
17 23
39 55

```


## Dry Run
**Input:**
```
n = 2
A = [[1, 2],
     [3, 4]]
B = [[5, 6],
     [7, 8]]
```

**Row-Major Calculation:**
```
C[0][0] = 1*5 + 2*7 = 19
C[0][1] = 1*6 + 2*8 = 22
C[1][0] = 3*5 + 4*7 = 43
C[1][1] = 3*6 + 4*8 = 50
```

**Result (Row-Major):**
```
19 22
43 50
```

**Column-Major Calculation:**
```
C[0][0] = 1*5 + 2*6 = 17
C[0][1] = 1*7 + 2*8 = 23
C[1][0] = 3*5 + 4*6 = 39
C[1][1] = 3*7 + 4*8 = 55
```

**Result (Column-Major):**
```
17 23
39 55
```

**Conclusion:**
Dynamic memory allows flexible matrix size.
Row-Major traversal is faster because memory in C++ is stored row-wise (contiguously).
