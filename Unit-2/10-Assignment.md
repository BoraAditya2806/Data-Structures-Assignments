# Assignment 10

## Problem Statement
Write a C++ program to split a singly linked list into **two sublists** — one for the front half and one for the back half.  
If the number of elements is odd, the extra element should go in the **front list**.

**Example:**  
FrontBackSplit() on `{2, 3, 5, 7, 11}` should yield `{2, 3, 5}` and `{7, 11}`.

The program must handle all edge cases such as:
- Length = 0 or 1 (no split possible)
- Length = 2 or 3 (boundary conditions)
- General cases for even/odd lengths

---

## Pseudocode

1. Define a node structure containing `data` and a pointer to the `next` node.  
2. Create a function to insert nodes at the end of the list.  
3. Implement a function `FrontBackSplit_agb()` that:
   - Uses **two pointers (slow and fast)**.
   - Moves `fast` by two nodes and `slow` by one node.
   - When `fast` reaches the end, `slow` will be at the midpoint.
   - Split the list at the midpoint.
4. Display both sublists.

---

## Code (C++)

```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node *next;
    Node(int val) {
        data = val;
        next = nullptr;
    }
};

void insertEnd_agb(Node* &head, int val) {
    Node *newNode = new Node(val);
    if (!head) {
        head = newNode;
        return;
    }
    Node *temp = head;
    while (temp->next)
        temp = temp->next;
    temp->next = newNode;
}

void display_agb(Node *head) {
    Node *temp = head;
    cout << "{";
    while (temp) {
        cout << temp->data;
        if (temp->next)
            cout << ", ";
        temp = temp->next;
    }
    cout << "}";
}

void FrontBackSplit_agb(Node *source, Node* &frontRef, Node* &backRef) {
    if (source == nullptr || source->next == nullptr) {
        frontRef = source;
        backRef = nullptr;
        return;
    }

    Node *slow = source;
    Node *fast = source->next;

    while (fast != nullptr) {
        fast = fast->next;
        if (fast != nullptr) {
            slow = slow->next;
            fast = fast->next;
        }
    }

    frontRef = source;
    backRef = slow->next;
    slow->next = nullptr;
}

int main() {
    Node *head = nullptr;
    int n, val;

    cout << "Enter number of elements: ";
    cin >> n;

    cout << "Enter elements: ";
    for (int i = 0; i < n; i++) {
        cin >> val;
        insertEnd_agb(head, val);
    }

    Node *front = nullptr, *back = nullptr;

    FrontBackSplit_agb(head, front, back);

    cout << "\nFront List: ";
    display_agb(front);

    cout << "\nBack List: ";
    display_agb(back);

    cout << endl;
    return 0;
}
```

---

## Input
```
Enter number of elements: 5
Enter elements: 2 3 5 7 11
```

## Output
```
Front List: {2, 3, 5}
Back List: {7, 11}
```

---

## Dry Run (Tabular Form)

| Step | List | slow | fast | Action |
|------|------|------|------|--------|
| 1 | {2,3,5,7,11} | 2 | 3 | Start |
| 2 | {2,3,5,7,11} | 3 | 5 | Move pointers |
| 3 | {2,3,5,7,11} | 5 | 11 | Split here |
| 4 | Front = {2,3,5}, Back = {7,11} |  |  | Done |

---

## Conclusion
The program successfully splits a singly linked list into **front** and **back halves** using the **fast-slow pointer method**.  
It correctly handles **odd/even length** lists and **boundary conditions**.
