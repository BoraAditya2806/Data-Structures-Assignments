# Assignment 1

## Problem Statement

Write a C++ program to build a simple **Stock Price Tracker** that maintains a history of daily stock prices using a **Stack implemented via Linked List**.
The program should allow users to record, view, and remove the most recent stock prices.

### Operations:

1. **record(price)** – Add a new stock price (an integer) to the stack.
2. **remove()** – Remove and return the most recent price (top of the stack).
3. **latest()** – Return the most recent stock price without removing it.
4. **isEmpty()** – Check if there are no prices recorded.

---

## Pseudocode

1. Define a Node structure with `data` (price) and `next` pointer.
2. Define a Stack class with functions:

   * `record_agb(int price)` → Push a new price onto the stack using `malloc`.
   * `remove_agb()` → Pop the top price from the stack.
   * `latest_agb()` → Display the top price without removing it.
   * `isEmpty_agb()` → Return true if stack is empty.
3. Display menu-driven operations for user input.
4. Ensure proper dynamic memory allocation check (`if (ptr == NULL)`).

---

## Code (C++)

```cpp
#include <iostream>
#include <cstdlib>
using namespace std;

struct Node {
    int data;
    Node *next;
};

class StockStack {
    Node *top;
public:
    StockStack() { top = NULL; }

    bool isEmpty_agb() {
        return top == NULL;
    }

    void record_agb(int price) {
        Node *newNode = (Node*)malloc(sizeof(Node));
        if (newNode == NULL) {
            cout << "Memory allocation failed! Exiting program." << endl;
            exit(1);
        }
        newNode->data = price;
        newNode->next = top;
        top = newNode;
        cout << "Recorded price: " << price << endl;
    }

    void remove_agb() {
        if (isEmpty_agb()) {
            cout << "No prices to remove." << endl;
            return;
        }
        Node *temp = top;
        cout << "Removed latest price: " << top->data << endl;
        top = top->next;
        free(temp);
    }

    void latest_agb() {
        if (isEmpty_agb()) {
            cout << "No prices recorded yet." << endl;
            return;
        }
        cout << "Latest price: " << top->data << endl;
    }

    void display_agb() {
        if (isEmpty_agb()) {
            cout << "No prices recorded." << endl;
            return;
        }
        cout << "Stock Price History (latest first): ";
        Node *temp = top;
        while (temp != NULL) {
            cout << temp->data << " ";
            temp = temp->next;
        }
        cout << endl;
    }
};

int main() {
    StockStack s;
    int choice, price;
    do {
        cout << "\n1. Record Price\n2. Remove Latest Price\n3. View Latest Price\n4. Display All Prices\n5. Check if Empty\n0. Exit\nEnter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter stock price: ";
                cin >> price;
                s.record_agb(price);
                break;
            case 2:
                s.remove_agb();
                break;
            case 3:
                s.latest_agb();
                break;
            case 4:
                s.display_agb();
                break;
            case 5:
                cout << (s.isEmpty_agb() ? "Stack is empty." : "Stack is not empty.") << endl;
                break;
            case 0:
                cout << "Exiting program." << endl;
                break;
            default:
                cout << "Invalid choice." << endl;
        }
    } while (choice != 0);

    return 0;
}
```

---

## Input Example

```
1. Record Price
2. Remove Latest Price
3. View Latest Price
4. Display All Prices
5. Check if Empty
0. Exit

Enter your choice: 1
Enter stock price: 1500
Recorded price: 1500

Enter your choice: 1
Enter stock price: 1550
Recorded price: 1550

Enter your choice: 4
Stock Price History (latest first): 1550 1500

Enter your choice: 3
Latest price: 1550

Enter your choice: 2
Removed latest price: 1550

Enter your choice: 4
Stock Price History (latest first): 1500
```

---

## Dry Run (Step-by-Step)

| Step | Operation    | Stack State (Top First) | Output                                   |
| ---- | ------------ | ----------------------- | ---------------------------------------- |
| 1    | record(1500) | 1500                    | Recorded price: 1500                     |
| 2    | record(1550) | 1550 → 1500             | Recorded price: 1550                     |
| 3    | latest()     | 1550 → 1500             | Latest price: 1550                       |
| 4    | remove()     | 1500                    | Removed latest price: 1550               |
| 5    | display()    | 1500                    | Stock Price History (latest first): 1500 |

---

## Conclusion

The program successfully implements a **Stack using a Linked List**.
It supports all stack operations — record, remove, latest, display, and check-empty.
