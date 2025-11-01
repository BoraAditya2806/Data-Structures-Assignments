# Assignment 6

## Problem Statement
Write a C++ program to store a binary number using a **doubly linked list**. Implement the following functions:  
a) Calculate and display the **1’s complement** and **2’s complement** of the stored binary number.  
b) Perform **addition of two binary numbers** represented using doubly linked lists and display the result.


## Pseudocode

1. Define a doubly linked list node structure with `data`, `prev`, and `next`.  
2. Input a binary number as a string and store each bit in the doubly linked list.  
3. To calculate 1’s complement:  
   - Traverse the list and replace each `0` with `1` and vice versa.  
4. To calculate 2’s complement:  
   - Start from the least significant bit.  
   - Copy bits until you encounter the first `1`, then invert the remaining bits.  
5. To add two binary numbers:  
   - Traverse both lists from the end toward the start.  
   - Add corresponding bits with carry.  
   - Create a new linked list for the sum.  
6. Display all results.


## Code (C++)
```cpp
#include <iostream>
#include <string>
using namespace std;

struct Node {
    int data;
    Node *prev, *next;
};

Node* insertEnd_agb(Node* head, int val) {
    Node* newNode = new Node{val, nullptr, nullptr};
    if (!head) return newNode;
    Node* temp = head;
    while (temp->next) temp = temp->next;
    temp->next = newNode;
    newNode->prev = temp;
    return head;
}

void display_agb(Node* head) {
    while (head) {
        cout << head->data;
        head = head->next;
    }
    cout << endl;
}

Node* createFromString_agb(string s) {
    Node* head = nullptr;
    for (char c : s) {
        head = insertEnd_agb(head, c - '0');
    }
    return head;
}

Node* onesComplement_agb(Node* head) {
    Node* temp = head;
    while (temp) {
        temp->data = (temp->data == 0) ? 1 : 0;
        temp = temp->next;
    }
    return head;
}

Node* twosComplement_agb(Node* head) {
    Node* temp = head;
    while (temp->next) temp = temp->next;
    bool carry = true;
    while (temp) {
        if (carry) {
            if (temp->data == 0) {
                temp->data = 1;
                carry = false;
            } else {
                temp->data = 0;
            }
        }
        temp = temp->prev;
    }
    if (carry) {
        Node* newNode = new Node{1, nullptr, head};
        head->prev = newNode;
        head = newNode;
    }
    return head;
}

Node* addBinary_agb(Node* a, Node* b) {
    Node* p = a, *q = b, *result = nullptr;
    while (p->next) p = p->next;
    while (q->next) q = q->next;

    int carry = 0;
    while (p || q || carry) {
        int sum = carry;
        if (p) { sum += p->data; p = p->prev; }
        if (q) { sum += q->data; q = q->prev; }
        carry = sum / 2;
        result = insertEnd_agb(result, sum % 2);
    }

    Node* prev = nullptr;
    Node* curr = result;
    Node* next = nullptr;
    while (curr) {
        next = curr->next;
        curr->next = prev;
        curr->prev = next;
        prev = curr;
        curr = next;
    }
    result = prev;

    return result;
}

int main() {
    string bin1, bin2;
    cout << "Enter a binary number: ";
    cin >> bin1;
    Node* head = createFromString_agb(bin1);

    cout << "\nOriginal Binary: ";
    display_agb(head);

    Node* ones = createFromString_agb(bin1);
    ones = onesComplement_agb(ones);
    cout << "1's Complement: ";
    display_agb(ones);

    Node* twos = createFromString_agb(bin1);
    twos = onesComplement_agb(twos);
    twos = twosComplement_agb(twos);
    cout << "2's Complement: ";
    display_agb(twos);

    cout << "\nEnter another binary number for addition: ";
    cin >> bin2;
    Node* head2 = createFromString_agb(bin2);

    Node* sum = addBinary_agb(head, head2);
    cout << "\nSum: ";
    display_agb(sum);

    return 0;
}
```

## Input
```
Enter a binary number: 10110
Enter another binary number for addition: 1101
```

## Output
```
Original Binary: 10110
1's Complement: 01001
2's Complement: 01010

Sum: 100011
```

## Dry Run

| Step | Operation | Result |
|------|------------|---------|
| 1 | Input binary number = 10110 | Stored in doubly linked list |
| 2 | 1’s complement → Flip all bits | 01001 |
| 3 | 2’s complement → Add 1 to LSB of 1’s complement | 01010 |
| 4 | Second binary = 1101 | Stored in linked list |
| 5 | Add both numbers (10110 + 1101) | 100011 |

**Final Results:**  
- 1’s Complement: 01001  
- 2’s Complement: 01010  
- Sum: 100011

## Conclusion
The program successfully demonstrates binary number manipulation using a **doubly linked list**, performing 1’s complement, 2’s complement, and binary addition efficiently through node traversal and carry management.
