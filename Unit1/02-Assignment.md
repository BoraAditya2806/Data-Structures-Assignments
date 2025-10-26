# Assignment 2: Magic Square of Order 'n' (Even & Odd)

## Code (C++)
```cpp
#include <iostream>
#include <iomanip>
using namespace std;

// Function to generate magic square for odd order
void magicSquareOdd_agb(int n, int square[50][50]) {
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
void magicSquareDoublyEven_agb(int n, int square[50][50]) {
    int i, j;
    int num = 1;
    for (i = 0; i < n; i++) {
        for (j = 0; j < n; j++) {
            square[i][j] = num++;
        }
    }

    // Change specific cells
    for (i = 0; i < n; i++) {
        for (j = 0; j < n; j++) {
            if (((i % 4 == j % 4) || (i % 4 + j % 4 == 3)))
                square[i][j] = n * n + 1 - square[i][j];
        }
    }
}

// Function to generate magic square for singly even order (n % 4 == 2)
void magicSquareSinglyEven_agb(int n, int square[50][50]) {
    int half = n / 2;
    int subSquareSize = half * half;
    int temp[50][50];
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

    // Swap left columns
    int k = (n - 2) / 4;
    for (int i = 0; i < half; i++) {
        for (int j = 0; j < k; j++) {
            swap(square[i][j], square[i + half][j]);
        }
        for (int j = n - k + 1; j < n; j++) {
            swap(square[i][j], square[i + half][j]);
        }
    }
}

// Function to print magic square
void printSquare_agb(int n, int square[50][50]) {
    int magicConstant = n * (n * n + 1) / 2;
    cout << "\nMagic Square of order " << n << " (Sum = " << magicConstant << "):\n";
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cout << setw(4) << square[i][j];
        }
        cout << endl;
    }
}

// Main function
int main() {
    int n;
    int square[50][50] = {0};
    cout << "Enter order of magic square (n): ";
    cin >> n;

    if (n < 3) {
        cout << "Magic square not possible for n < 3.";
        return 0;
    }

    if (n % 2 == 1)
        magicSquareOdd_agb(n, square);
    else if (n % 4 == 0)
        magicSquareDoublyEven_agb(n, square);
    else
        magicSquareSinglyEven_agb(n, square);

    printSquare_agb(n, square);

    return 0;
}
```

## Sample Output
```
Enter order of magic square (n): 3

Magic Square of order 3 (Sum = 15):
  8  1  6
  3  5  7
  4  9  2
```

```
Enter order of magic square (n): 4

Magic Square of order 4 (Sum = 34):
 16   2   3  13
  5  11  10   8
  9   7   6  12
  4  14  15   1
```

## Pseudocode
1. **Input `n`**
   - Read the order of magic square.

2. **Check type of `n`**
   - If `n` is **odd**, use `magicSquareOdd_agb`.
   - If `n % 4 == 0`, use `magicSquareDoublyEven_agb`.
   - If `n % 4 == 2`, use `magicSquareSinglyEven_agb`.

3. **Magic Square Construction Rules**
   - **Odd n:**  
     - Start from top middle.  
     - Place next number diagonally up-right.  
     - If out of bounds, wrap around.
   - **Doubly Even (n % 4 == 0):**  
     - Fill matrix sequentially.  
     - In specific diagonal patterns, replace number with `(n*n + 1 - current)`.
   - **Singly Even (n % 4 == 2):**  
     - Divide matrix into 4 smaller squares.  
     - Fill each with smaller magic squares and rearrange using swapping pattern.

4. **Display Square**
   - Print square in tabular format.
   - Display magic constant = `n * (n² + 1) / 2`.

## Dry Run
**Example:**  
`n = 3`

| Step | Action | Result |
|------|---------|---------|
| 1 | Start position (0, 1) | Place 1 |
| 2 | Move up-right → (2, 2) | Place 2 |
| 3 | Move up-right → (1, 0) | Place 3 |
| 4 | Move up-right → (0, 1) (filled) → move down → (2, 1) | Place 4 |
| ... | Continue | ... |

**Final Magic Square (n=3):**
```
8 1 6
3 5 7
4 9 2
```

**Magic Constant = 15**

---

*All rows, columns, and diagonals sum to 15 for n = 3 and to 34 for n = 4.*
