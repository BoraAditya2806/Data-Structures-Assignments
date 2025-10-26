# Assignment 4: Sparse Matrix Representation and Simple Transpose 

## Code (C++)
```cpp
#include <iostream>
using namespace std;

// Structure for sparse matrix representation
struct Sparse_agb {
    int **data; // compact form [row, col, value]
    int rows, cols, nonZero;
};

// Function to create compact representation
Sparse_agb *createSparse_agb(int **matrix, int rows, int cols) {
    int count = 0;

    // Count non-zero elements
    for (int i = 0; i < rows; i++)
        for (int j = 0; j < cols; j++)
            if (matrix[i][j] != 0)
                count++;

    // Allocate memory for Sparse matrix
    Sparse_agb *s = (Sparse_agb *)malloc(sizeof(Sparse_agb));
    if (s == NULL) {
        cout << "Memory allocation failed! Exiting program." << endl;
        exit(1);
    }

    s->rows = rows;
    s->cols = cols;
    s->nonZero = count;

    s->data = (int **)malloc((count + 1) * sizeof(int *)); // +1 for header
    if (s->data == NULL) {
        cout << "Memory allocation failed! Exiting program." << endl;
        exit(1);
    }

    for (int i = 0; i <= count; i++) {
        s->data[i] = (int *)malloc(3 * sizeof(int));
        if (s->data[i] == NULL) {
            cout << "Memory allocation failed for row " << i << ". Exiting program." << endl;
            exit(1);
        }
    }

    // Header row
    s->data[0][0] = rows;
    s->data[0][1] = cols;
    s->data[0][2] = count;

    int k = 1;
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            if (matrix[i][j] != 0) {
                s->data[k][0] = i;
                s->data[k][1] = j;
                s->data[k][2] = matrix[i][j];
                k++;
            }
        }
    }

    return s;
}

// Function to display sparse matrix
void displaySparse_agb(Sparse_agb *s) {
    cout << "\nCompact Sparse Matrix Representation:\n";
    cout << "Row\tCol\tValue\n";
    for (int i = 0; i <= s->nonZero; i++) {
        cout << s->data[i][0] << "\t" << s->data[i][1] << "\t" << s->data[i][2] << endl;
    }
}

// Function to compute simple transpose
Sparse_agb *simpleTranspose_agb(Sparse_agb *s) {
    Sparse_agb *t = (Sparse_agb *)malloc(sizeof(Sparse_agb));
    if (t == NULL) {
        cout << "Memory allocation failed! Exiting program." << endl;
        exit(1);
    }

    t->rows = s->cols;
    t->cols = s->rows;
    t->nonZero = s->nonZero;

    t->data = (int **)malloc((s->nonZero + 1) * sizeof(int *));
    if (t->data == NULL) {
        cout << "Memory allocation failed! Exiting program." << endl;
        exit(1);
    }

    for (int i = 0; i <= s->nonZero; i++) {
        t->data[i] = (int *)malloc(3 * sizeof(int));
        if (t->data[i] == NULL) {
            cout << "Memory allocation failed for transpose row " << i << ". Exiting program." << endl;
            exit(1);
        }
    }

    // Header
    t->data[0][0] = s->cols;
    t->data[0][1] = s->rows;
    t->data[0][2] = s->nonZero;

    int k = 1;
    for (int col = 0; col < s->cols; col++) {
        for (int i = 1; i <= s->nonZero; i++) {
            if (s->data[i][1] == col) {
                t->data[k][0] = s->data[i][1];
                t->data[k][1] = s->data[i][0];
                t->data[k][2] = s->data[i][2];
                k++;
            }
        }
    }

    return t;
}

int main() {
    int rows, cols;
    cout << "Enter number of rows and columns: ";
    cin >> rows >> cols;

    // Dynamic matrix creation
    int **matrix = (int **)malloc(rows * sizeof(int *));
    if (matrix == NULL) {
        cout << "Memory allocation failed! Exiting program." << endl;
        exit(1);
    }
    for (int i = 0; i < rows; i++) {
        matrix[i] = (int *)malloc(cols * sizeof(int));
        if (matrix[i] == NULL) {
            cout << "Memory allocation failed for row " << i << ". Exiting program." << endl;
            exit(1);
        }
    }

    cout << "Enter elements of matrix:\n";
    for (int i = 0; i < rows; i++)
        for (int j = 0; j < cols; j++)
            cin >> matrix[i][j];

    Sparse_agb *sparse = createSparse_agb(matrix, rows, cols);
    displaySparse_agb(sparse);

    Sparse_agb *transpose = simpleTranspose_agb(sparse);
    cout << "\nTranspose of Sparse Matrix:\n";
    displaySparse_agb(transpose);

    // Free memory
    for (int i = 0; i < rows; i++)
        free(matrix[i]);
    free(matrix);

    for (int i = 0; i <= sparse->nonZero; i++)
        free(sparse->data[i]);
    free(sparse->data);
    free(sparse);

    for (int i = 0; i <= transpose->nonZero; i++)
        free(transpose->data[i]);
    free(transpose->data);
    free(transpose);

    return 0;
}
```

## Input
```
Enter number of rows and columns: 3 3
Enter elements of matrix:
0 0 5
0 8 0
9 0 0
```

## Sample Output
```
Compact Sparse Matrix Representation:
Row     Col     Value
3       3       3
0       2       5
1       1       8
2       0       9

Transpose of Sparse Matrix:
Row     Col     Value
3       3       3
0       2       9
1       1       8
2       0       5
```

## Pseudocode
1. **Input rows and cols**

2. **Allocate matrix using malloc()**
   - If memory fails → exit.

3. **Input matrix elements**

4. **Count non-zero elements**

5. **Create sparse compact form [row, col, value]**

6. **Display compact form**

7. **Generate transpose by swapping row & column**

8. **Display transpose**

9. **Free all allocated memory**

## Expected Output
```
Original Sparse Representation and Transpose both stored in compact form.
Memory-efficient representation achieved.
```

## Dry Run
**Input:**
```
Matrix =
0 0 5
0 8 0
9 0 0
```

**Non-zero count = 3**

**Compact Form:**
| Row | Col | Val |
|------|------|------|
| 3 | 3 | 3 |
| 0 | 2 | 5 |
| 1 | 1 | 8 |
| 2 | 0 | 9 |

**Transpose:**
| Row | Col | Val |
|------|------|------|
| 3 | 3 | 3 |
| 0 | 2 | 9 |
| 1 | 1 | 8 |
| 2 | 0 | 5 |

 **Conclusion:**
Sparse matrices save space by storing only non-zero elements.
Dynamic memory allocation makes it flexible for any matrix size.
