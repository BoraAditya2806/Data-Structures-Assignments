# Assignment 3
## Problem Statement

Write a program to implement multiple stacks (more than two stacks) using a single array and perform the following operations:

- A. Push
- B. Pop
- C. Stack Overflow
- D. Stack Underflow
- E. Display

---

## Pseudocode

```
DEFINE MAX_SIZE = total array size
DEFINE NUM_STACKS = number of stacks to create

CLASS MultipleStacks:
    array[MAX_SIZE]
    top[NUM_STACKS]
    stackSize = MAX_SIZE / NUM_STACKS

    FUNCTION __init__():
        FOR i = 0 TO NUM_STACKS-1:
            top[i] = i * stackSize - 1  // Initialize each stack's top

    FUNCTION getBaseIndex(stackNum):
        RETURN stackNum * stackSize

    FUNCTION getTopLimit(stackNum):
        RETURN (stackNum + 1) * stackSize - 1

    FUNCTION isFull(stackNum):
        RETURN top[stackNum] >= getTopLimit(stackNum)

    FUNCTION isEmpty(stackNum):
        RETURN top[stackNum] < getBaseIndex(stackNum)

    FUNCTION push(stackNum, value):
        IF isFull(stackNum):
            PRINT "Stack Overflow for Stack", stackNum
            RETURN FALSE
        top[stackNum]++
        array[top[stackNum]] = value
        PRINT "Pushed", value, "to Stack", stackNum
        RETURN TRUE

    FUNCTION pop(stackNum):
        IF isEmpty(stackNum):
            PRINT "Stack Underflow for Stack", stackNum
            RETURN -1
        value = array[top[stackNum]]
        top[stackNum]--
        PRINT "Popped", value, "from Stack", stackNum
        RETURN value

    FUNCTION peek(stackNum):
        IF isEmpty(stackNum):
            PRINT "Stack", stackNum, "is empty"
            RETURN -1
        RETURN array[top[stackNum]]

    FUNCTION display(stackNum):
        IF isEmpty(stackNum):
            PRINT "Stack", stackNum, "is empty"
            RETURN
        PRINT elements from base to top

    FUNCTION displayAll():
        FOR i = 0 TO NUM_STACKS-1:
            display(i)

MAIN:
    CREATE MultipleStacks object
    LOOP:
        DISPLAY menu
        GET choice
        SWITCH choice:
            CASE Push: GET stackNum and value, CALL push()
            CASE Pop: GET stackNum, CALL pop()
            CASE Display: GET stackNum, CALL display()
            CASE DisplayAll: CALL displayAll()
            CASE Exit
```

---

## C++ Code

```cpp
#include <iostream>
#include <iomanip>
using namespace std;

class MultipleStacks {
private:
    int* arr;           
    int* top;           
    int MAX_SIZE;       
    int NUM_STACKS;     
    int stackSize;      

    // Get base index for a stack
    int getBaseIndex_agb(int stackNum) {
        return stackNum * stackSize;
    }

    // Get top limit for a stack
    int getTopLimit_agb(int stackNum) {
        return (stackNum + 1) * stackSize - 1;
    }

public:
    // Constructor
    MultipleStacks(int totalSize, int numStacks) {
        MAX_SIZE = totalSize;
        NUM_STACKS = numStacks;
        stackSize = MAX_SIZE / NUM_STACKS;

        arr = new int[MAX_SIZE];
        top = new int[NUM_STACKS];

        for (int i = 0; i < NUM_STACKS; i++) {
            top[i] = getBaseIndex_agb(i) - 1;
        }

        cout << "Created " << NUM_STACKS << " stacks with size "
             << stackSize << " each." << endl;
    }

    // Check if a stack is full
    bool isFull_agb(int stackNum) {
        if (stackNum < 0 || stackNum >= NUM_STACKS) {
            cout << "Invalid stack number!" << endl;
            return true;
        }
        return top[stackNum] >= getTopLimit_agb(stackNum);
    }

    // Check if a stack is empty
    bool isEmpty_agb(int stackNum) {
        if (stackNum < 0 || stackNum >= NUM_STACKS) {
            cout << "Invalid stack number!" << endl;
            return true;
        }
        return top[stackNum] < getBaseIndex_agb(stackNum);
    }

    // Push element to a specific stack
    bool push_agb(int stackNum, int value) {
        if (stackNum < 0 || stackNum >= NUM_STACKS) {
            cout << "Invalid stack number!" << endl;
            return false;
        }

        if (isFull_agb(stackNum)) {
            cout << "*** STACK OVERFLOW *** Stack " << stackNum
                 << " is full! Cannot push " << value << endl;
            return false;
        }

        top[stackNum]++;
        arr[top[stackNum]] = value;
        cout << "Pushed " << value << " to Stack " << stackNum << endl;
        return true;
    }

    // Pop element from a specific stack
    int pop_agb(int stackNum) {
        if (stackNum < 0 || stackNum >= NUM_STACKS) {
            cout << "Invalid stack number!" << endl;
            return -1;
        }

        if (isEmpty_agb(stackNum)) {
            cout << "*** STACK UNDERFLOW *** Stack " << stackNum
                 << " is empty! Cannot pop." << endl;
            return -1;
        }

        int value = arr[top[stackNum]];
        top[stackNum]--;
        cout << "Popped " << value << " from Stack " << stackNum << endl;
        return value;
    }

    // Peek top element of a stack
    int peek_agb(int stackNum) {
        if (stackNum < 0 || stackNum >= NUM_STACKS) {
            cout << "Invalid stack number!" << endl;
            return -1;
        }

        if (isEmpty_agb(stackNum)) {
            cout << "Stack " << stackNum << " is empty!" << endl;
            return -1;
        }

        return arr[top[stackNum]];
    }

    // Display a specific stack
    void display_agb(int stackNum) {
        if (stackNum < 0 || stackNum >= NUM_STACKS) {
            cout << "Invalid stack number!" << endl;
            return;
        }

        if (isEmpty_agb(stackNum)) {
            cout << "Stack " << stackNum << " is EMPTY!" << endl;
            return;
        }

        cout << "\n--- Stack " << stackNum << " ---" << endl;
        cout << "Elements (Bottom to Top): ";
        for (int i = getBaseIndex_agb(stackNum); i <= top[stackNum]; i++) {
            cout << arr[i] << " ";
        }
        cout << "\nTop element: " << arr[top[stackNum]] << endl;
        cout << "Size: " << (top[stackNum] - getBaseIndex_agb(stackNum) + 1)
             << "/" << stackSize << endl;
    }

    // Display all stacks
    void displayAll_agb() {
        cout << "\n========== ALL STACKS ==========" << endl;
        for (int i = 0; i < NUM_STACKS; i++) {
            display_agb(i);
        }
        cout << "================================" << endl;
    }

    // Display array structure
    void displayArrayStructure_agb() {
        cout << "\n--- Array Structure ---" << endl;
        cout << "Index: ";
        for (int i = 0; i < MAX_SIZE; i++) {
            cout << setw(4) << i;
        }
        cout << "\nValue: ";
        for (int i = 0; i < MAX_SIZE; i++) {
            bool occupied = false;
            for (int s = 0; s < NUM_STACKS; s++) {
                if (i >= getBaseIndex_agb(s) && i <= top[s]) {
                    occupied = true;
                    break;
                }
            }
            if (occupied)
                cout << setw(4) << arr[i];
            else
                cout << setw(4) << "-";
        }
        cout << "\nStack: ";
        for (int i = 0; i < MAX_SIZE; i++) {
            cout << setw(4) << (i / stackSize);
        }
        cout << endl;
    }

    // Destructor
    ~MultipleStacks() {
        delete[] arr;
        delete[] top;
    }
};

int main() {
    int totalSize, numStacks;

    cout << "Enter total array size: ";
    cin >> totalSize;
    cout << "Enter number of stacks: ";
    cin >> numStacks;

    MultipleStacks ms(totalSize, numStacks);

    int choice, stackNum, value;

    do {
        cout << "\n===== Multiple Stacks Menu =====" << endl;
        cout << "1. Push" << endl;
        cout << "2. Pop" << endl;
        cout << "3. Peek" << endl;
        cout << "4. Display a stack" << endl;
        cout << "5. Display all stacks" << endl;
        cout << "6. Display array structure" << endl;
        cout << "7. Check if stack is full (Overflow check)" << endl;
        cout << "8. Check if stack is empty (Underflow check)" << endl;
        cout << "9. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter stack number (0-" << numStacks-1 << "): ";
                cin >> stackNum;
                cout << "Enter value to push: ";
                cin >> value;
                ms.push_agb(stackNum, value);
                break;

            case 2:
                cout << "Enter stack number (0-" << numStacks-1 << "): ";
                cin >> stackNum;
                ms.pop_agb(stackNum);
                break;

            case 3:
                cout << "Enter stack number (0-" << numStacks-1 << "): ";
                cin >> stackNum;
                value = ms.peek_agb(stackNum);
                if (value != -1) {
                    cout << "Top element of Stack " << stackNum
                         << ": " << value << endl;
                }
                break;

            case 4:
                cout << "Enter stack number (0-" << numStacks-1 << "): ";
                cin >> stackNum;
                ms.display_agb(stackNum);
                break;

            case 5:
                ms.displayAll_agb();
                break;

            case 6:
                ms.displayArrayStructure_agb();
                break;

            case 7:
                cout << "Enter stack number (0-" << numStacks-1 << "): ";
                cin >> stackNum;
                if (ms.isFull_agb(stackNum)) {
                    cout << "Stack " << stackNum << " is FULL (Overflow condition)!" << endl;
                } else {
                    cout << "Stack " << stackNum << " is NOT full." << endl;
                }
                break;

            case 8:
                cout << "Enter stack number (0-" << numStacks-1 << "): ";
                cin >> stackNum;
                if (ms.isEmpty_agb(stackNum)) {
                    cout << "Stack " << stackNum << " is EMPTY (Underflow condition)!" << endl;
                } else {
                    cout << "Stack " << stackNum << " is NOT empty." << endl;
                }
                break;

            case 9:
                cout << "Exiting... Thank you!" << endl;
                break;

            default:
                cout << "Invalid choice!" << endl;
        }
    } while (choice != 9);

    return 0;
}

```

---

## Input/Output

### Sample Run:

```
Enter total array size: 15
Enter number of stacks: 3
Created 3 stacks with size 5 each.

===== Multiple Stacks Menu =====
1. Push
2. Pop
3. Peek
4. Display a stack
5. Display all stacks
6. Display array structure
7. Check if stack is full (Overflow check)
8. Check if stack is empty (Underflow check)
9. Exit
Enter your choice: 1
Enter stack number (0-2): 0
Enter value to push: 10
Pushed 10 to Stack 0

Enter your choice: 1
Enter stack number (0-2): 0
Enter value to push: 20
Pushed 20 to Stack 0

Enter your choice: 1
Enter stack number (0-2): 1
Enter value to push: 100
Pushed 100 to Stack 1

Enter your choice: 1
Enter stack number (0-2): 2
Enter value to push: 500
Pushed 500 to Stack 2

Enter your choice: 5

========== ALL STACKS ==========

--- Stack 0 ---
Elements (Bottom to Top): 10 20
Top element: 20
Size: 2/5

--- Stack 1 ---
Elements (Bottom to Top): 100
Top element: 100
Size: 1/5

--- Stack 2 ---
Elements (Bottom to Top): 500
Top element: 500
Size: 1/5
================================

Enter your choice: 6

--- Array Structure ---
Index:    0   1   2   3   4   5   6   7   8   9  10  11  12  13  14
Value:   10  20   -   -   - 100   -   -   -   - 500   -   -   -   -
Stack:    0   0   0   0   0   1   1   1   1   1   2   2   2   2   2

Enter your choice: 1
Enter stack number (0-2): 0
Enter value to push: 30
Pushed 30 to Stack 0

Enter your choice: 1
Enter stack number (0-2): 0
Enter value to push: 40
Pushed 40 to Stack 0

Enter your choice: 1
Enter stack number (0-2): 0
Enter value to push: 50
Pushed 50 to Stack 0

Enter your choice: 1
Enter stack number (0-2): 0
Enter value to push: 60
*** STACK OVERFLOW *** Stack 0 is full! Cannot push 60

Enter your choice: 7
Enter stack number (0-2): 0
Stack 0 is FULL (Overflow condition)!

Enter your choice: 2
Enter stack number (0-2): 1
Popped 100 from Stack 1

Enter your choice: 2
Enter stack number (0-2): 1
*** STACK UNDERFLOW *** Stack 1 is empty! Cannot pop.

Enter your choice: 8
Enter stack number (0-2): 1
Stack 1 is EMPTY (Underflow condition)!

Enter your choice: 9
Exiting... Thank you!
```

---

## Dry Run

### Configuration:

- Total Array Size: 15
- Number of Stacks: 3
- Stack Size each: 15/3 = 5

### Initial State:

```
Array indices:  0  1  2  3  4  5  6  7  8  9 10 11 12 13 14
Stack regions:  [  Stack 0  ] [  Stack 1  ] [  Stack 2  ]
                0  1  2  3  4  5  6  7  8  9 10 11 12 13 14

top[0] = -1 (base at 0)
top[1] = 4  (base at 5)
top[2] = 9  (base at 10)
```

### Operation 1: push(0, 10)

```
Check: isFull(0)? top[0]=−1 < limit=4 → NO
top[0]++ → top[0] = 0
arr[0] = 10

Array: [10, -, -, -, -, -, -, -, -, -, -, -, -, -, -]
top[0] = 0
```

### Operation 2: push(0, 20)

```
Check: isFull(0)? top[0]=0 < limit=4 → NO
top[0]++ → top[0] = 1
arr[1] = 20

Array: [10, 20, -, -, -, -, -, -, -, -, -, -, -, -, -]
top[0] = 1
```

### Operation 3: push(1, 100)

```
Check: isFull(1)? top[1]=4 < limit=9 → NO
top[1]++ → top[1] = 5
arr[5] = 100

Array: [10, 20, -, -, -, 100, -, -, -, -, -, -, -, -, -]
top[1] = 5
```

### Operation 4: push(0, 30), push(0, 40), push(0, 50)

```
After these operations:
Array: [10, 20, 30, 40, 50, 100, -, -, -, -, -, -, -, -, -]
top[0] = 4 (FULL - reached limit)
```

### Operation 5: push(0, 60) - **OVERFLOW**

```
Check: isFull(0)? top[0]=4 >= limit=4 → YES
Display: "STACK OVERFLOW"
No push performed
```

### Operation 6: pop(1)

```
Check: isEmpty(1)? top[1]=5 >= base=5 → NO
value = arr[5] = 100
top[1]-- → top[1] = 4
Return 100

Array: [10, 20, 30, 40, 50, -, -, -, -, -, -, -, -, -, -]
top[1] = 4
```

### Operation 7: pop(1) - **UNDERFLOW**

```
Check: isEmpty(1)? top[1]=4 < base=5 → YES
Display: "STACK UNDERFLOW"
Return -1
```

---

## Conclusion

This program successfully implements **Multiple Stacks in a Single Array** with the following key achievements:

1. **Efficient Space Utilization**:

   - Single array divided equally among multiple stacks
   - Each stack gets `totalSize / numStacks` positions
   - Memory is allocated contiguously

2. **Array Division Strategy**:

   ```
   Stack 0: indices [0 to stackSize-1]
   Stack 1: indices [stackSize to 2*stackSize-1]
   Stack 2: indices [2*stackSize to 3*stackSize-1]
   ...and so on
   ```

3. **Core Operations Implemented**:

   - **Push**: O(1) - Direct index access
   - **Pop**: O(1) - Direct index access
   - **Overflow Detection**: Checks if top reaches upper limit
   - **Underflow Detection**: Checks if top goes below base
   - **Display**: O(k) where k is elements in that stack

4. **Boundary Management**:

   - Base index for stack i: `i * stackSize`
   - Top limit for stack i: `(i+1) * stackSize - 1`
   - Top pointer initialized one position before base

5. **Error Handling**:

   - **Overflow**: Attempted push when `top[i] >= topLimit[i]`
   - **Underflow**: Attempted pop when `top[i] < baseIndex[i]`
   - Invalid stack number validation

6. **Limitations**:
   - Fixed size allocation (one stack full while others empty wastes space)
   - Cannot dynamically redistribute space
   - Solution: Use dynamic allocation or linked representation

**Time Complexity**: O(1) for all operations  
**Space Complexity**: O(n) where n is total array size  
**Applications**: Memory-efficient implementation when multiple stacks needed simultaneously
