# Assignment 1
## Problem Statement

Write a program to build a simple stock price tracker that keeps a history of daily stock prices entered by the user. To allow users to go back and view or remove the most recent price, implement a stack using a linked list to store these integer prices.

Implement the following operations:

- `record(price)` – Add a new stock price (an integer) to the stack
- `remove()` – Remove and return the most recent price (top of the stack)
- `latest()` – Return the most recent stock price without removing it
- `isEmpty()` – Check if there are no prices recorded

---

## Pseudocode

```
CLASS Node:
    data: integer
    next: pointer to Node

CLASS StockPriceTracker:
    top: pointer to Node

    FUNCTION __init__():
        top = NULL

    FUNCTION record(price):
        newNode = CREATE Node with price
        newNode.next = top
        top = newNode
        PRINT "Price recorded"

    FUNCTION remove():
        IF isEmpty():
            PRINT "Stack underflow"
            RETURN -1
        temp = top
        price = top.data
        top = top.next
        DELETE temp
        RETURN price

    FUNCTION latest():
        IF isEmpty():
            PRINT "No prices recorded"
            RETURN -1
        RETURN top.data

    FUNCTION isEmpty():
        RETURN top == NULL

    FUNCTION display():
        IF isEmpty():
            PRINT "No prices to display"
            RETURN
        current = top
        WHILE current != NULL:
            PRINT current.data
            current = current.next

MAIN:
    CREATE StockPriceTracker object
    LOOP:
        DISPLAY menu
        GET choice
        SWITCH choice:
            CASE 1: GET price, CALL record(price)
            CASE 2: CALL remove()
            CASE 3: CALL latest()
            CASE 4: CALL isEmpty()
            CASE 5: CALL display()
            CASE 6: EXIT
```

---

## C++ Code

```cpp
#include <iostream>
using namespace std;

class Node {
public:
    int data;
    Node* next;

    Node(int price) {
        data = price;
        next = NULL;
    }
};

class StockPriceTracker {
private:
    Node* top;

public:
    StockPriceTracker() {
        top = NULL;
    }

    // Record a new stock price
    void record_agb(int price) {
        Node* newNode = new Node(price);
        newNode->next = top;
        top = newNode;
        cout << "Price $" << price << " recorded successfully!" << endl;
    }

    // Remove and return the most recent price
    int remove_agb() {
        if (isEmpty_agb()) {
            cout << "Stack Underflow! No prices to remove." << endl;
            return -1;
        }
        Node* temp = top;
        int price = top->data;
        top = top->next;
        delete temp;
        cout << "Removed price: $" << price << endl;
        return price;
    }

    // Return the most recent price without removing
    int latest_agb() {
        if (isEmpty_agb()) {
            cout << "No prices recorded yet!" << endl;
            return -1;
        }
        return top->data;
    }

    // Check if stack is empty
    bool isEmpty_agb() {
        return top == NULL;
    }

    // Display all prices
    void display_agb() {
        if (isEmpty_agb()) {
            cout << "No prices to display!" << endl;
            return;
        }

        cout << "\n--- Stock Price History (Most Recent First) ---" << endl;
        Node* current = top;
        int position = 1;
        while (current != NULL) {
            cout << position++ << ". $" << current->data << endl;
            current = current->next;
        }
        cout << "-----------------------------------------------" << endl;
    }

    // Destructor
    ~StockPriceTracker() {
        while (!isEmpty_agb()) {
            remove_agb();
        }
    }
};

int main() {
    StockPriceTracker tracker;
    int choice, price;

    do {
        cout << "\n===== Stock Price Tracker Menu =====" << endl;
        cout << "1. Record new price" << endl;
        cout << "2. Remove latest price" << endl;
        cout << "3. View latest price" << endl;
        cout << "4. Check if empty" << endl;
        cout << "5. Display all prices" << endl;
        cout << "6. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter stock price: $";
                cin >> price;
                tracker.record_agb(price);
                break;

            case 2:
                tracker.remove_agb();
                break;

            case 3:
                price = tracker.latest_agb();
                if (price != -1) {
                    cout << "Latest price: $" << price << endl;
                }
                break;

            case 4:
                if (tracker.isEmpty_agb()) {
                    cout << "Stack is EMPTY!" << endl;
                } else {
                    cout << "Stack is NOT empty!" << endl;
                }
                break;

            case 5:
                tracker.display_agb();
                break;

            case 6:
                cout << "Exiting... Thank you!" << endl;
                break;

            default:
                cout << "Invalid choice! Please try again." << endl;
        }
    } while (choice != 6);

    return 0;
}

```

---

## Input/Output

### Sample Run:

```
===== Stock Price Tracker Menu =====
1. Record new price
2. Remove latest price
3. View latest price
4. Check if empty
5. Display all prices
6. Exit
Enter your choice: 4
Stack is EMPTY!

===== Stock Price Tracker Menu =====
1. Record new price
2. Remove latest price
3. View latest price
4. Check if empty
5. Display all prices
6. Exit
Enter your choice: 1
Enter stock price: $150
Price $150 recorded successfully!

===== Stock Price Tracker Menu =====
1. Record new price
2. Remove latest price
3. View latest price
4. Check if empty
5. Display all prices
6. Exit
Enter your choice: 1
Enter stock price: $175
Price $175 recorded successfully!

===== Stock Price Tracker Menu =====
1. Record new price
2. Remove latest price
3. View latest price
4. Check if empty
5. Display all prices
6. Exit
Enter your choice: 1
Enter stock price: $160
Price $160 recorded successfully!

===== Stock Price Tracker Menu =====
1. Record new price
2. Remove latest price
3. View latest price
4. Check if empty
5. Display all prices
6. Exit
Enter your choice: 5

--- Stock Price History (Most Recent First) ---
1. $160
2. $175
3. $150
-----------------------------------------------

===== Stock Price Tracker Menu =====
1. Record new price
2. Remove latest price
3. View latest price
4. Check if empty
5. Display all prices
6. Exit
Enter your choice: 3
Latest price: $160

===== Stock Price Tracker Menu =====
1. Record new price
2. Remove latest price
3. View latest price
4. Check if empty
5. Display all prices
6. Exit
Enter your choice: 2
Removed price: $160

===== Stock Price Tracker Menu =====
1. Record new price
2. Remove latest price
3. View latest price
4. Check if empty
5. Display all prices
6. Exit
Enter your choice: 5

--- Stock Price History (Most Recent First) ---
1. $175
2. $150
-----------------------------------------------

===== Stock Price Tracker Menu =====
1. Record new price
2. Remove latest price
3. View latest price
4. Check if empty
5. Display all places
6. Exit
Enter your choice: 6
Exiting... Thank you!
```

---

## Dry Run

### Initial State:

```
top = NULL
```

### Operation 1: record(150)

```
Step 1: Create newNode with data = 150
Step 2: newNode->next = NULL
Step 3: top = newNode

Stack: [150] <- top
```

### Operation 2: record(175)

```
Step 1: Create newNode with data = 175
Step 2: newNode->next = top (points to 150)
Step 3: top = newNode

Stack: [175] <- top -> [150] -> NULL
```

### Operation 3: record(160)

```
Step 1: Create newNode with data = 160
Step 2: newNode->next = top (points to 175)
Step 3: top = newNode

Stack: [160] <- top -> [175] -> [150] -> NULL
```

### Operation 4: latest()

```
Step 1: Check if isEmpty() - NO
Step 2: Return top->data = 160

Stack: [160] <- top -> [175] -> [150] -> NULL (unchanged)
```

### Operation 5: remove()

```
Step 1: Check if isEmpty() - NO
Step 2: temp = top (points to 160)
Step 3: price = top->data = 160
Step 4: top = top->next (now points to 175)
Step 5: delete temp

Stack: [175] <- top -> [150] -> NULL
```

### Operation 6: display()

```
Step 1: current = top (175)
Step 2: Print 175, current = current->next (150)
Step 3: Print 150, current = current->next (NULL)
Step 4: Loop ends

Output:
1. $175
2. $150
```

---

## Conclusion

This program successfully implements a **Stack using Linked List** for tracking stock prices with the following key features:

1. **Dynamic Memory Management**: Uses linked list for efficient memory utilization without fixed size constraints

2. **LIFO Principle**: Follows Last-In-First-Out principle where the most recent price is always accessible at the top

3. **Core Operations Implemented**:

   - `record()`: O(1) - Constant time insertion at the beginning
   - `remove()`: O(1) - Constant time deletion from the beginning
   - `latest()`: O(1) - Constant time access to top element
   - `isEmpty()`: O(1) - Constant time check
   - `display()`: O(n) - Linear time for traversal

4. **Error Handling**: Properly handles underflow conditions when trying to access/remove from an empty stack

5. **Real-world Application**: Demonstrates practical use of stack data structure in financial tracking systems where recent data is frequently accessed

6. **Memory Efficiency**: Destructor ensures proper memory deallocation preventing memory leaks

**Time Complexity Summary**:

- Push/Record: O(1)
- Pop/Remove: O(1)
- Peek/Latest: O(1)
- Display: O(n)

**Space Complexity**: O(n) where n is the number of prices stored
