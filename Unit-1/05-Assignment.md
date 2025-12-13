# Assignment 5: Fast Transpose of a Sparse Matrix 

## Pseudocode
1. **Input matrix size and elements**

2. **Count non-zero elements**

3. **Store in compact triplet form [row, col, value]**

4. **Initialize row_terms[] and starting_pos[]**
   - `row_terms[j]` → number of elements in column *j*
   - `starting_pos[j]` → position to place next element in transposed matrix

5. **For each non-zero element:**
   - Place `(col, row, value)` at correct transposed position.

6. **Display transposed compact form.**

7. **Free all allocated memory.**
## Code (C++)
```cpp
#include <iostream>
using namespace std;

struct Sparse_agb {
    int **data; 
    int rows, cols, nonZero;
};

Sparse_agb *createSparse_agb(int **matrix, int rows, int cols) {
    int count = 0;

    for (int i = 0; i < rows; i++)
        for (int j = 0; j < cols; j++)
            if (matrix[i][j] != 0)
                count++;

    Sparse_agb *s = (Sparse_agb *)malloc(sizeof(Sparse_agb));
    if (s == NULL) {
        cout << "Memory allocation failed! Exiting program." << endl;
        exit(1);
    }

    s->rows = rows;
    s->cols = cols;
    s->nonZero = count;

    s->data = (int **)malloc((count + 1) * sizeof(int *));
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

void displaySparse_agb(Sparse_agb *s) {
    cout << "\nCompact Sparse Matrix Representation:\n";
    cout << "Row\tCol\tValue\n";
    for (int i = 0; i <= s->nonZero; i++) {
        cout << s->data[i][0] << "\t" << s->data[i][1] << "\t" << s->data[i][2] << endl;
    }
}

Sparse_agb *fastTranspose_agb(Sparse_agb *s) {
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

    
    t->data[0][0] = s->cols;
    t->data[0][1] = s->rows;
    t->data[0][2] = s->nonZero;

    int *row_terms = (int *)malloc(s->cols, sizeof(int));
    int *starting_pos = (int *)malloc(s->cols, sizeof(int));

    if (row_terms == NULL || starting_pos == NULL) {
        cout << "Memory allocation failed for helper arrays! Exiting program." << endl;
        exit(1);
    }

    for (int i = 1; i <= s->nonZero; i++)
        row_terms[s->data[i][1]]++;

    starting_pos[0] = 1;
    for (int i = 1; i < s->cols; i++)
        starting_pos[i] = starting_pos[i - 1] + row_terms[i - 1];

    for (int i = 1; i <= s->nonZero; i++) {
        int col = s->data[i][1];
        int pos = starting_pos[col];
        t->data[pos][0] = s->data[i][1];
        t->data[pos][1] = s->data[i][0];
        t->data[pos][2] = s->data[i][2];
        starting_pos[col]++;
    }

    free(row_terms);
    free(starting_pos);
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

    Sparse_agb *transpose = fastTranspose_agb(sparse);
    cout << "\nFast Transpose of Sparse Matrix:\n";
    displaySparse_agb(transpose);

    
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

## Output
```
Compact Sparse Matrix Representation:
Row     Col     Value
3       3       3
0       2       5
1       1       8
2       0       9

Fast Transpose of Sparse Matrix:
Row     Col     Value
3       3       3
0       2       9
1       1       8
2       0       5
```




## Dry Run
**Input:**
```
Matrix =
0 0 5
0 8 0
9 0 0
```

**Compact Form:**
| Row | Col | Val |
|------|------|------|
| 3 | 3 | 3 |
| 0 | 2 | 5 |
| 1 | 1 | 8 |
| 2 | 0 | 9 |

**row_terms = [1, 1, 1]**
**starting_pos = [1, 2, 3]**

After Transpose:
| Row | Col | Val |
|------|------|------|
| 3 | 3 | 3 |
| 0 | 2 | 9 |
| 1 | 1 | 8 |
| 2 | 0 | 5 |

**Conclusion:**
Fast transpose computes transpose directly by using position mapping.
Efficient for large sparse matrices with less time and memory usage.
