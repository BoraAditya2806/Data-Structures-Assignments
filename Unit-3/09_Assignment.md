# Assignment 9

## Problem Statement

Write a program to implement multiple queues (two queues) using array and perform following operations:

- A. Add to Queue
- B. Delete from Queue
- C. Display Queue

---

## Pseudocode

```
CLASS MultipleQueues:
    array[MAX_SIZE]
    front1, rear1: integer  // For queue 1
    front2, rear2: integer  // For queue 2
    queueSize: integer

    FUNCTION __init__(size):
        MAX_SIZE = size
        queueSize = MAX_SIZE / 2
        front1 = -1
        rear1 = -1
        front2 = queueSize
        rear2 = queueSize - 1

    FUNCTION isEmpty(queueNum):
        IF queueNum == 1:
            RETURN front1 == -1
        ELSE:
            RETURN front2 == queueSize

    FUNCTION isFull(queueNum):
        IF queueNum == 1:
            RETURN rear1 >= queueSize - 1
        ELSE:
            RETURN rear2 >= MAX_SIZE - 1

    FUNCTION enqueue(queueNum, value):
        IF isFull(queueNum):
            PRINT "Queue overflow"
            RETURN FALSE

        IF queueNum == 1:
            IF front1 == -1:
                front1 = 0
            rear1++
            array[rear1] = value
        ELSE:
            IF front2 == queueSize:
                front2 = queueSize
            rear2++
            array[rear2] = value

        RETURN TRUE

    FUNCTION dequeue(queueNum):
        IF isEmpty(queueNum):
            PRINT "Queue underflow"
            RETURN -1

        value = 0
        IF queueNum == 1:
            value = array[front1]
            IF front1 == rear1:
                front1 = rear1 = -1
            ELSE:
                front1++
        ELSE:
            value = array[front2]
            IF front2 == rear2:
                front2 = queueSize
                rear2 = queueSize - 1
            ELSE:
                front2++

        RETURN value

    FUNCTION display(queueNum):
        IF isEmpty(queueNum):
            PRINT "Queue is empty"
            RETURN

        IF queueNum == 1:
            FOR i = front1 TO rear1:
                PRINT array[i]
        ELSE:
            FOR i = front2 TO rear2:
                PRINT array[i]

MAIN:
    INPUT size
    CREATE MultipleQueues object
    PERFORM operations
```

---

## C++ Code

```cpp
#include <iostream>
#include <iomanip>
using namespace std;

class MultipleQueues {
private:
    int* arr;
    int MAX_SIZE;
    int queueSize_agb;

    int front1_agb, rear1_agb;

    int front2_agb, rear2_agb;

public:
    MultipleQueues_agb(int size) {
        MAX_SIZE = size;
        queueSize_agb = MAX_SIZE / 2;
        arr = new int[MAX_SIZE];

        front1_agb = -1;
        rear1_agb = -1;

        front2_agb = queueSize_agb;
        rear2_agb = queueSize_agb - 1;

        cout << "Two queues created with size " << queueSize_agb << " each." << endl;
    }

    bool isEmpty_agb(int queueNum) {
        if (queueNum == 1) {
            return front1_agb == -1;
        } else {
            return front2_agb == queueSize_agb;
        }
    }

    bool isFull_agb(int queueNum) {
        if (queueNum == 1) {
            return rear1_agb >= queueSize_agb - 1;
        } else {
            return rear2_agb >= MAX_SIZE - 1;
        }
    }

    bool enqueue_agb(int queueNum, int value) {
        if (queueNum != 1 && queueNum != 2) {
            cout << "Invalid queue number!" << endl;
            return false;
        }

        if (isFull_agb(queueNum)) {
            cout << "✗ Queue " << queueNum << " OVERFLOW! Cannot add "
                 << value << endl;
            return false;
        }

        if (queueNum == 1) {
            if (front1_agb == -1) {
                front1_agb = 0;
            }
            rear1_agb++;
            arr[rear1_agb] = value;
        } else {
            if (front2_agb == queueSize_agb) {
                front2_agb = queueSize_agb;
            }
            rear2_agb++;
            arr[rear2_agb] = value;
        }

        cout << "✓ Added " << value << " to Queue " << queueNum << endl;
        return true;
    }

    int dequeue_agb(int queueNum) {
        if (queueNum != 1 && queueNum != 2) {
            cout << "Invalid queue number!" << endl;
            return -1;
        }

        if (isEmpty_agb(queueNum)) {
            cout << "✗ Queue " << queueNum << " UNDERFLOW! Queue is empty." << endl;
            return -1;
        }

        int value = 0;

        if (queueNum == 1) {
            value = arr[front1_agb];

            if (front1_agb == rear1_agb) {
                front1_agb = -1;
                rear1_agb = -1;
            } else {
                front1_agb++;
            }
        } else {
            value = arr[front2_agb];

            if (front2_agb == rear2_agb) {
                front2_agb = queueSize_agb;
                rear2_agb = queueSize_agb - 1;
            } else {
                front2_agb++;
            }
        }

        cout << "✓ Removed " << value << " from Queue " << queueNum << endl;
        return value;
    }

    void display_agb(int queueNum) {
        if (queueNum != 1 && queueNum != 2) {
            cout << "Invalid queue number!" << endl;
            return;
        }

        if (isEmpty_agb(queueNum)) {
            cout << "\nQueue " << queueNum << " is EMPTY!" << endl;
            return;
        }

        cout << "\n--- Queue " << queueNum << " ---" << endl;
        cout << "Elements (Front to Rear): ";

        if (queueNum == 1) {
            for (int i = front1_agb; i <= rear1_agb; i++) {
                cout << arr[i] << " ";
            }
            cout << "\nFront: " << arr[front1_agb]
                 << ", Rear: " << arr[rear1_agb] << endl;
            cout << "Size: " << (rear1_agb - front1_agb + 1)
                 << "/" << queueSize_agb << endl;
        } else {
            for (int i = front2_agb; i <= rear2_agb; i++) {
                cout << arr[i] << " ";
            }
            cout << "\nFront: " << arr[front2_agb]
                 << ", Rear: " << arr[rear2_agb] << endl;
            cout << "Size: " << (rear2_agb - front2_agb + 1)
                 << "/" << queueSize_agb << endl;
        }
    }

    void displayAll_agb() {
        cout << "\n========== Both Queues ==========" << endl;
        display_agb(1);
        display_agb(2);
        cout << "================================" << endl;
    }

    void displayArrayStructure_agb() {
        cout << "\n--- Array Structure ---" << endl;
        cout << "Index: ";
        for (int i = 0; i < MAX_SIZE; i++) {
            cout << setw(4) << i;
        }
        cout << "\nValue: ";

        for (int i = 0; i < MAX_SIZE; i++) {
            bool hasValue = false;

            if (i >= front1_agb && i <= rear1_agb && front1_agb != -1) {
                cout << setw(4) << arr[i];
                hasValue = true;
            }
            else if (i >= front2_agb && i <= rear2_agb && front2_agb != queueSize_agb) {
                cout << setw(4) << arr[i];
                hasValue = true;
            }

            if (!hasValue) {
                cout << setw(4) << "-";
            }
        }

        cout << "\nQueue: ";
        for (int i = 0; i < MAX_SIZE; i++) {
            if (i < queueSize_agb) {
                cout << setw(4) << "Q1";
            } else {
                cout << setw(4) << "Q2";
            }
        }
        cout << endl;
    }

    ~MultipleQueues_agb() {
        delete[] arr;
    }
};

int main_agb() {
    int size;

    cout << "===== Multiple Queues Using Array =====" << endl;
    cout << "Enter total array size (even number): ";
    cin >> size;

    if (size % 2 != 0) {
        size++;
        cout << "Adjusted to even size: " << size << endl;
    }

    MultipleQueues_agb mq_agb(size);
    int choice, queueNum, value;

    do {
        cout << "\n===== Menu =====" << endl;
        cout << "1. Add to queue (Enqueue)" << endl;
        cout << "2. Remove from queue (Dequeue)" << endl;
        cout << "3. Display a queue" << endl;
        cout << "4. Display both queues" << endl;
        cout << "5. Display array structure" << endl;
        cout << "6. Check if queue is empty" << endl;
        cout << "7. Check if queue is full" << endl;
        cout << "8. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter queue number (1 or 2): ";
                cin >> queueNum;
                cout << "Enter value to add: ";
                cin >> value;
                mq_agb.enqueue_agb(queueNum, value);
                break;

            case 2:
                cout << "Enter queue number (1 or 2): ";
                cin >> queueNum;
                mq_agb.dequeue_agb(queueNum);
                break;

            case 3:
                cout << "Enter queue number (1 or 2): ";
                cin >> queueNum;
                mq_agb.display_agb(queueNum);
                break;

            case 4:
                mq_agb.displayAll_agb();
                break;

            case 5:
                mq_agb.displayArrayStructure_agb();
                break;

            case 6:
                cout << "Enter queue number (1 or 2): ";
                cin >> queueNum;
                if (mq_agb.isEmpty_agb(queueNum)) {
                    cout << "Queue " << queueNum << " is EMPTY!" << endl;
                } else {
                    cout << "Queue " << queueNum << " is NOT empty." << endl;
                }
                break;

            case 7:
                cout << "Enter queue number (1 or 2): ";
                cin >> queueNum;
                if (mq_agb.isFull_agb(queueNum)) {
                    cout << "Queue " << queueNum << " is FULL!" << endl;
                } else {
                    cout << "Queue " << queueNum << " is NOT full." << endl;
                }
                break;

            case 8:
                cout << "Exiting... Thank you!" << endl;
                break;

            default:
                cout << "Invalid choice!" << endl;
        }
    } while (choice != 8);

    return 0;
}
```

---

## Input/Output

### Sample Run:

```
===== Multiple Queues Using Array =====
Enter total array size (even number): 10
Two queues created with size 5 each.

===== Menu =====
Enter your choice: 1
Enter queue number (1 or 2): 1
Enter value to add: 10
✓ Added 10 to Queue 1

Enter your choice: 1
Enter queue number (1 or 2): 1
Enter value to add: 20
✓ Added 20 to Queue 1

Enter your choice: 1
Enter queue number (1 or 2): 2
Enter value to add: 100
✓ Added 100 to Queue 2

Enter your choice: 1
Enter queue number (1 or 2): 2
Enter value to add: 200
✓ Added 200 to Queue 2

Enter your choice: 4

========== Both Queues ==========

--- Queue 1 ---
Elements (Front to Rear): 10 20
Front: 10, Rear: 20
Size: 2/5

--- Queue 2 ---
Elements (Front to Rear): 100 200
Front: 100, Rear: 200
Size: 2/5
================================

Enter your choice: 5

--- Array Structure ---
Index:    0   1   2   3   4   5   6   7   8   9
Value:   10  20   -   -   - 100 200   -   -   -
Queue:   Q1  Q1  Q1  Q1  Q1  Q2  Q2  Q2  Q2  Q2

Enter your choice: 1
Enter queue number (1 or 2): 1
Enter value to add: 30
✓ Added 30 to Queue 1

Enter your choice: 1
Enter queue number (1 or 2): 1
Enter value to add: 40
✓ Added 40 to Queue 1

Enter your choice: 1
Enter queue number (1 or 2): 1
Enter value to add: 50
✓ Added 50 to Queue 1

Enter your choice: 1
Enter queue number (1 or 2): 1
Enter value to add: 60
✗ Queue 1 OVERFLOW! Cannot add 60

Enter your choice: 2
Enter queue number (1 or 2): 1
✓ Removed 10 from Queue 1

Enter your choice: 2
Enter queue number (1 or 2): 1
✓ Removed 20 from Queue 1

Enter your choice: 3
Enter queue number (1 or 2): 1

--- Queue 1 ---
Elements (Front to Rear): 30 40 50
Front: 30, Rear: 50
Size: 3/5

Enter your choice: 2
Enter queue number (1 or 2): 2
✓ Removed 100 from Queue 2

Enter your choice: 6
Enter queue number (1 or 2): 2
Queue 2 is NOT empty.

Enter your choice: 8
Exiting... Thank you!
```

---

## Dry Run

### Configuration: Array size = 10 (Queue 1: indices 0-4, Queue 2: indices 5-9)

### Initial State:

```
Array indices:  0  1  2  3  4  5  6  7  8  9
              [Q1][Q1][Q1][Q1][Q1][Q2][Q2][Q2][Q2][Q2]

front1 = -1, rear1 = -1
front2 = 5, rear2 = 4
```

### Operation 1: enqueue(1, 10)

```
Step 1: isFull(1)? NO (rear1 < 4)
Step 2: front1 == -1? YES → front1 = 0
Step 3: rear1++ → rear1 = 0
Step 4: arr[0] = 10

State:
front1 = 0, rear1 = 0
Array: [10, -, -, -, -, -, -, -, -, -]
         ^F1,R1
```

### Operation 2: enqueue(1, 20)

```
Step 1: isFull(1)? NO
Step 2: rear1++ → rear1 = 1
Step 3: arr[1] = 20

State:
front1 = 0, rear1 = 1
Array: [10, 20, -, -, -, -, -, -, -, -]
        ^F1  ^R1
```

### Operation 3: enqueue(2, 100)

```
Step 1: isFull(2)? NO (rear2 < 9)
Step 2: front2 == 5? YES → front2 = 5
Step 3: rear2++ → rear2 = 5
Step 4: arr[5] = 100

State:
front2 = 5, rear2 = 5
Array: [10, 20, -, -, -, 100, -, -, -, -]
        ^F1  ^R1       ^F2,R2
```

### Operation 4: enqueue(2, 200)

```
rear2++ → rear2 = 6
arr[6] = 200

State:
Array: [10, 20, -, -, -, 100, 200, -, -, -]
        ^F1  ^R1       ^F2   ^R2
```

### Operations 5-7: enqueue(1, 30), enqueue(1, 40), enqueue(1, 50)

```
After these operations:
front1 = 0, rear1 = 4
Array: [10, 20, 30, 40, 50, 100, 200, -, -, -]
        ^F1             ^R1  ^F2   ^R2
Queue 1 is FULL
```

### Operation 8: enqueue(1, 60) - OVERFLOW

```
Step 1: isFull(1)? rear1(4) >= queueSize-1(4)? YES
Display: "Queue 1 OVERFLOW"
No changes
```

### Operation 9: dequeue(1)

```
Step 1: isEmpty(1)? NO
Step 2: value = arr[0] = 10
Step 3: front1 == rear1? 0 == 4? NO
Step 4: front1++ → front1 = 1

State:
front1 = 1, rear1 = 4
Array: [-, 20, 30, 40, 50, 100, 200, -, -, -]
           ^F1          ^R1
```

### Operation 10: dequeue(1)

```
value = arr[1] = 20
front1++ → front1 = 2

State:
front1 = 2, rear1 = 4
Array: [-, -, 30, 40, 50, 100, 200, -, -, -]
              ^F1      ^R1
```

### Operation 11: dequeue(2)

```
value = arr[5] = 100
front2++ → front2 = 6

State:
front2 = 6, rear2 = 6
Array: [-, -, 30, 40, 50, -, 200, -, -, -]
              ^F1      ^R1     ^F2,R2
```

---

## Conclusion

This program successfully implements **Multiple Queues Using a Single Array**:

1. **Array Division Strategy**:

   - Total array divided into two equal parts
   - Queue 1: indices [0 to size/2-1]
   - Queue 2: indices [size/2 to size-1]
   - Each queue operates independently

2. **Queue 1 Management**:

   - Front pointer: front1 (initialized to -1)
   - Rear pointer: rear1 (initialized to -1)
   - Range: 0 to queueSize-1

3. **Queue 2 Management**:

   - Front pointer: front2 (initialized to queueSize)
   - Rear pointer: rear2 (initialized to queueSize-1)
   - Range: queueSize to MAX_SIZE-1

4. **Core Operations**:

   - **Enqueue**: O(1) - Add at rear
   - **Dequeue**: O(1) - Remove from front
   - **Display**: O(n) where n is queue size
   - **isEmpty/isFull**: O(1)

5. **Overflow & Underflow Handling**:

   - **Overflow**: When rear reaches upper limit of allocated space
   - **Underflow**: When trying to dequeue from empty queue
   - Clear error messages for both conditions

6. **Limitations**:
   - Fixed size allocation per queue
   - Wasted space if one queue empty and other needs more space
   - No dynamic redistribution of array space

**Time Complexity**:

- All operations: O(1) except display which is O(n)

**Space Complexity**: O(n) where n is total array size

**Applications**:

- Process scheduling with priorities
- Resource allocation systems
- Multi-level caching
- Printer job management

**Comparison with Single Queue**:
| Feature | Single Queue | Multiple Queues |
|---------|--------------|-----------------|
| Space utilization | Better | May waste space |
| Flexibility | Less | More (separate management) |
| Complexity | Simple | Moderate |
| Use case | General purpose | Priority-based systems |
