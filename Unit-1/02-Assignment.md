# Assignment 2: Magic Square of Order 'n' (Even & Odd) 

## Pseudocode
1. **Input n**
   - Read order of magic square.

2. **Dynamically allocate memory**
   - Use `malloc()` to allocate 2D array of size `n x n`.
   - If allocation fails, display an error and exit.

3. **Check order type**
   - If n is **odd** → use `magicSquareOdd_agb()`
   - If n % 4 == 0 → use `magicSquareDoublyEven_agb()`
   - If n % 4 == 2 → use `magicSquareSinglyEven_agb()`

4. **Generate and Print Magic Square**
   - Display matrix with rows, columns, and diagonals having the same sum.

5. **Free Memory**
   - Use `free()` for all dynamically allocated arrays.

   
## Code (C++)
```cpp
#include <iostream>
#include <iomanip>
using namespace std;

void magicSquareOdd_agb(int n, int **square) {
    int num = 1;
    int i = 0, j = n / 2;
    while (num <= n * n) {
        square[i][j] = num++;
        i--;
        j++;

        if (num % n == 1) { 
            i += 2;
            j--;
        } else {
            if (i < 0)
                i = n - 1;
            if (j == n)
                j = 0;
        }
    }
}

void magicSquareDoublyEven_agb(int n, int **square) {
    int num = 1;
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            square[i][j] = num++;

    // Change specific cells
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if ((i % 4 == j % 4) || (i % 4 + j % 4 == 3))
                square[i][j] = n * n + 1 - square[i][j];
        }
    }
}


void magicSquareSinglyEven_agb(int n, int **square) {
    int half = n / 2;
    int subSquareSize = half * half;

    // allocate temp 2D array
    int **temp = (int **)malloc(half * sizeof(int *));
    if (temp == NULL) {
        cout << "Memory allocation failed! Exiting program.\n";
        exit(1);
    }
    for (int i = 0; i < half; i++) {
        temp[i] = (int *)malloc(half * sizeof(int));
        if (temp[i] == NULL) {
            cout << "Memory allocation failed for sub-square row. Exiting.\n";
            exit(1);
        }
    }

    magicSquareOdd_agb(half, temp);

    int add[] = {0, 2, 3, 1};
    for (int i = 0; i < half; i++) {
        for (int j = 0; j < half; j++) {
            for (int k = 0; k < 4; k++) {
                int row = i + (k / 2) * half;
                int col = j + (k % 2) * half;
                square[row][col] = temp[i][j] + add[k] * subSquareSize;
            }
        }
    }

    for (int i = 0; i < half; i++)
        free(temp[i]);
    free(temp);
}

void printSquare_agb(int n, int **square) {
    int magicConstant = n * (n * n + 1) / 2;
    cout << "\nMagic Square of order " << n << " (Sum = " << magicConstant << "):\n";
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++)
            cout << setw(4) << square[i][j];
        cout << endl;
    }
}

int main() {
    int n;
    cout << "Enter order of magic square (n): ";
    cin >> n;

    if (n < 3) {
        cout << "Magic square not possible for n < 3.";
        return 0;
    }

    int **square = (int **)malloc(n * sizeof(int *));
    if (square == NULL) {
        cout << "Memory allocation failed! Exiting program.\n";
        exit(1);
    }

    for (int i = 0; i < n; i++) {
        square[i] = (int *)malloc(n * sizeof(int));
        if (square[i] == NULL) {
            cout << "Memory allocation failed for row " << i << ". Exiting program.\n";
            exit(1);
        }
    }

    if (n % 2 == 1)
        magicSquareOdd_agb(n, square);
    else if (n % 4 == 0)
        magicSquareDoublyEven_agb(n, square);
    else
        magicSquareSinglyEven_agb(n, square);

   
    printSquare_agb(n, square);

   
    for (int i = 0; i < n; i++)
        free(square[i]);
    free(square);

    return 0;
}
```

## Input
```
Enter order of magic square (n): 3
```

## Output
```
Magic Square of order 3 (Sum = 15):
   8   1   6
   3   5   7
   4   9   2
```


## Dry Run
**Input:**
```
n = 3
```
| Step | Number | Current Row | Current Col                                       | Action / Rule Used        | New Row | New Col | Matrix after placement (nonzero = placed so far) |
| ---- | ------ | ----------- | ------------------------------------------------- | ------------------------- | ------- | ------- | ------------------------------------------------ |
| 1    | 1      | 0           | 1                                                 | Start at top middle       | 0       | 1       | 0 1 0<br>0 0 0<br>0 0 0                          |
| 2    | 2      | -1          | 2 → Wrap to (2,2)                                 | Move up-right (wrap)      | 2       | 2       | 0 1 0<br>0 0 0<br>0 0 2                          |
| 3    | 3      | 1           | 0                                                 | Move up-right             | 1       | 0       | 0 1 0<br>3 0 0<br>0 0 2                          |
| 4    | 4      | 0           | 1 (occupied) → Move down from prev (1,0) to (2,0) | Cell occupied → move down | 2       | 0       | 0 1 0<br>3 0 0<br>4 0 2                          |
| 5    | 5      | 1           | 1                                                 | Move up-right             | 1       | 1       | 0 1 0<br>3 5 0<br>4 0 2                          |
| 6    | 6      | 0           | 2                                                 | Move up-right             | 0       | 2       | 0 1 6<br>3 5 0<br>4 0 2                          |
| 7    | 7      | -1          | 3 → Wrap to (2,0) (occupied) → Move down → (1,2)  | Cell occupied → move down | 1       | 2       | 0 1 6<br>3 5 7<br>4 0 2                          |
| 8    | 8      | 0           | 0                                                 | Move up-right             | 0       | 0       | 8 1 6<br>3 5 7<br>4 0 2                          |
| 9    | 9      | -1          | 1 → Wrap to (2,1)                                 | Move up-right (wrap)      | 2       | 1       | 8 1 6<br>3 5 7<br>4 9 2                          |



**Final Matrix:**
| 8 | 1 | 6 |
|---|---|---|
| 3 | 5 | 7 |
| 4 | 9 | 2 |


 **Conclusion:**
Dynamic allocation allows flexible square size.
All rows, columns, and diagonals give the same sum, verifying it’s a **Magic Square**.
