# Assignment 8

## Problem Statement
Write a C++ program to implement **Bubble Sort** using a **Doubly Linked List**.  
The program should allow insertion of elements into a doubly linked list and sort them in **ascending order** using the bubble sort algorithm by swapping node data.

## Pseudocode

1. Define a structure `Node` with:
   - `data` → integer value
   - `next` → pointer to next node
   - `prev` → pointer to previous node
2. Insert elements into the doubly linked list.
3. Traverse the list multiple times, comparing adjacent nodes:
   - If the current node’s data is greater than the next node’s data, swap their values.
4. Repeat the process until the list is fully sorted.
5. Display the list before and after sorting.

## Code (C++)
```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node *next;
    Node *prev;
};

Node* createNode_agb(int data) {
    Node* newNode = new Node;
    newNode->data = data;
    newNode->next = nullptr;
    newNode->prev = nullptr;
    return newNode;
}

void insertEnd_agb(Node* &head, int data) {
    Node* newNode = createNode_agb(data);
    if (!head) {
        head = newNode;
        return;
    }
    Node* temp = head;
    while (temp->next) temp = temp->next;
    temp->next = newNode;
    newNode->prev = temp;
}

void display_agb(Node* head) {
    Node* temp = head;
    while (temp) {
        cout << temp->data << " ";
        temp = temp->next;
    }
    cout << endl;
}

void bubbleSort_agb(Node* head) {
    if (!head) return;
    bool swapped;
    Node* ptr1;
    Node* lptr = nullptr;

    do {
        swapped = false;
        ptr1 = head;

        while (ptr1->next != lptr) {
            if (ptr1->data > ptr1->next->data) {
                int temp = ptr1->data;
                ptr1->data = ptr1->next->data;
                ptr1->next->data = temp;
                swapped = true;
            }
            ptr1 = ptr1->next;
        }
        lptr = ptr1;
    } while (swapped);
}

int main() {
    Node* head = nullptr;
    int n, val;

    cout << "Enter number of elements: ";
    cin >> n;

    cout << "Enter " << n << " elements: ";
    for (int i = 0; i < n; i++) {
        cin >> val;
        insertEnd_agb(head, val);
    }

    cout << "\nOriginal List: ";
    display_agb(head);

    bubbleSort_agb(head);

    cout << "Sorted List (Ascending Order): ";
    display_agb(head);

    return 0;
}
```

## Input
```
Enter number of elements: 5
Enter 5 elements: 45 12 89 23 56
```

## Output
```
Original List: 45 12 89 23 56
Sorted List (Ascending Order): 12 23 45 56 89
```

## Dry Run 

| Pass | Comparison | Swap | List After Pass |
|------|-------------|------|------------------|
| 1 | (45,12), (12,89), (89,23), (23,56) | Yes | 12 45 23 56 89 |
| 2 | (12,45), (45,23), (23,56) | Yes | 12 23 45 56 89 |
| 3 | (12,23), (23,45) | No | 12 23 45 56 89 |

**Final Sorted List:**  
`12 23 45 56 89`

## Conclusion
The program successfully implements **Bubble Sort** using a **Doubly Linked List**.  
Nodes are traversed iteratively and adjacent data values are swapped until the list is sorted in ascending order.
