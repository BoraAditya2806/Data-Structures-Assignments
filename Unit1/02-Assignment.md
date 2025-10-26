# Assignment 2: Magic Square of Order 'n' (Even & Odd) 

## Code (C++)
```cpp
#include <iostream>
#include <iomanip>
using namespace std;

// Function to generate magic square for odd order
void magicSquareOdd_agb(int n, int **square) {
    int num = 1;
    int i = 0, j = n / 2; // start position

    while (num <= n * n) {
        square[i][j] = num++;
        i--;
        j++;

        if (num % n == 1) { // next row start
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

// Function to generate magic square for doubly even order (n % 4 == 0)
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

// Function to generate magic square for singly even order (n % 4 == 2)
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

    // Free temp
    for (int i = 0; i < half; i++)
        free(temp[i]);
    free(temp);
}

// Function to print magic square
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

    // Dynamic memory allocation for magic square
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

    // Generate magic square
    if (n % 2 == 1)
        magicSquareOdd_agb(n, square);
    else if (n % 4 == 0)
        magicSquareDoublyEven_agb(n, square);
    else
        magicSquareSinglyEven_agb(n, square);

    // Print the square
    printSquare_agb(n, square);

    // Free allocated memory
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

## Sample Output
```
Magic Square of order 3 (Sum = 15):
   8   1   6
   3   5   7
   4   9   2
```

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

## Expected Output
```
Magic Square of order 4 (Sum = 34):
 16   2   3  13
  5  11  10   8
  9   7   6  12
  4  14  15   1
```

## Dry Run
**Input:**
```
n = 3
```

**Step 1:** Start at top-middle → place 1.
**Step 2:** Move up-right → wrap around.
**Step 3:** Continue filling using rule.

**Final Matrix:**
| 8 | 1 | 6 |
|---|---|---|
| 3 | 5 | 7 |
| 4 | 9 | 2 |

 **Conclusion:**
Dynamic allocation allows flexible square size.
All rows, columns, and diagonals give the same sum, verifying it’s a **Magic Square**.
