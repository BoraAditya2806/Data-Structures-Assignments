# Assignment 9

## Problem Statement
Write a C++ program to **create a Doubly Linked List** and perform the following operations:  
1. **Insertion** — at beginning, end, and a specific position  
2. **Deletion** — from beginning, end, and a specific position  

The program should display the list after each operation.

## Pseudocode

1. Define a structure `Node` with:
   - `data` → integer value
   - `next` → pointer to next node
   - `prev` → pointer to previous node
2. Implement functions to:
   - Insert a node at the **beginning**
   - Insert a node at the **end**
   - Insert a node at a **specific position**
   - Delete a node from the **beginning**
   - Delete a node from the **end**
   - Delete a node from a **specific position**
3. Display the list after each operation.

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

void display_agb(Node* head) {
    Node* temp = head;
    while (temp) {
        cout << temp->data << " ";
        temp = temp->next;
    }
    cout << endl;
}

void insertBegin_agb(Node* &head, int data) {
    Node* newNode = createNode_agb(data);
    if (head) {
        newNode->next = head;
        head->prev = newNode;
    }
    head = newNode;
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

void insertPos_agb(Node* &head, int data, int pos) {
    if (pos <= 1 || !head) {
        insertBegin_agb(head, data);
        return;
    }
    Node* temp = head;
    for (int i = 1; i < pos - 1 && temp->next; i++)
        temp = temp->next;
    Node* newNode = createNode_agb(data);
    newNode->next = temp->next;
    newNode->prev = temp;
    if (temp->next) temp->next->prev = newNode;
    temp->next = newNode;
}

void deleteBegin_agb(Node* &head) {
    if (!head) return;
    Node* temp = head;
    head = head->next;
    if (head) head->prev = nullptr;
    delete temp;
}

void deleteEnd_agb(Node* &head) {
    if (!head) return;
    if (!head->next) {
        delete head;
        head = nullptr;
        return;
    }
    Node* temp = head;
    while (temp->next) temp = temp->next;
    temp->prev->next = nullptr;
    delete temp;
}

void deletePos_agb(Node* &head, int pos) {
    if (!head) return;
    if (pos == 1) {
        deleteBegin_agb(head);
        return;
    }
    Node* temp = head;
    for (int i = 1; i < pos && temp; i++)
        temp = temp->next;
    if (!temp) return;
    if (temp->next) temp->next->prev = temp->prev;
    if (temp->prev) temp->prev->next = temp->next;
    delete temp;
}

int main() {
    Node* head = nullptr;
    int choice, val, pos;

    do {
        cout << "\nMenu:\n1. Insert at Beginning\n2. Insert at End\n3. Insert at Position";
        cout << "\n4. Delete from Beginning\n5. Delete from End\n6. Delete from Position\n7. Display\n8. Exit\nEnter choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter value: ";
                cin >> val;
                insertBegin_agb(head, val);
                break;
            case 2:
                cout << "Enter value: ";
                cin >> val;
                insertEnd_agb(head, val);
                break;
            case 3:
                cout << "Enter value and position: ";
                cin >> val >> pos;
                insertPos_agb(head, val, pos);
                break;
            case 4:
                deleteBegin_agb(head);
                break;
            case 5:
                deleteEnd_agb(head);
                break;
            case 6:
                cout << "Enter position to delete: ";
                cin >> pos;
                deletePos_agb(head, pos);
                break;
            case 7:
                cout << "Current List: ";
                display_agb(head);
                break;
            case 8:
                cout << "Exiting...";
                break;
            default:
                cout << "Invalid choice!";
        }
    } while (choice != 8);

    return 0;
}
```

## Input && Output
```

Menu:
1. Insert at Beginning
2. Insert at End
3. Insert at Position
4. Delete from Beginning
5. Delete from End
6. Delete from Position
7. Display
8. Exit
Enter choice: 1
Enter value: 10

Menu:
1. Insert at Beginning
2. Insert at End
3. Insert at Position
4. Delete from Beginning
5. Delete from End
6. Delete from Position
7. Display
8. Exit
Enter choice: 2
Enter value: 20

Menu:
1. Insert at Beginning
2. Insert at End
3. Insert at Position
4. Delete from Beginning
5. Delete from End
6. Delete from Position
7. Display
8. Exit
Enter choice: 3
Enter value and position: 15 2

Menu:
1. Insert at Beginning
2. Insert at End
3. Insert at Position
4. Delete from Beginning
5. Delete from End
6. Delete from Position
7. Display
8. Exit
Enter choice: 7
Current List: 10 15 20 

Menu:
1. Insert at Beginning
2. Insert at End
3. Insert at Position
4. Delete from Beginning
5. Delete from End
6. Delete from Position
7. Display
8. Exit
Enter choice: 4

Menu:
1. Insert at Beginning
2. Insert at End
3. Insert at Position
4. Delete from Beginning
5. Delete from End
6. Delete from Position
7. Display
8. Exit
Enter choice: 7
Current List: 15 20 

Menu:
1. Insert at Beginning
2. Insert at End
3. Insert at Position
4. Delete from Beginning
5. Delete from End
6. Delete from Position
7. Display
8. Exit
Enter choice: 5

Menu:
1. Insert at Beginning
2. Insert at End
3. Insert at Position
4. Delete from Beginning
5. Delete from End
6. Delete from Position
7. Display
8. Exit
Enter choice: 7
Current List: 15 

Menu:
1. Insert at Beginning
2. Insert at End
3. Insert at Position
4. Delete from Beginning
5. Delete from End
6. Delete from Position
7. Display
8. Exit
Enter choice: 6
Enter position to delete: 1

Menu:
1. Insert at Beginning
2. Insert at End
3. Insert at Position
4. Delete from Beginning
5. Delete from End
6. Delete from Position
7. Display
8. Exit
Enter choice: 7
Current List: 

Menu:
1. Insert at Beginning
2. Insert at End
3. Insert at Position
4. Delete from Beginning
5. Delete from End
6. Delete from Position
7. Display
8. Exit
Enter choice: 8
Exiting...

```

## Dry Run (Step-by-Step in Tabular Form)

| Operation | Action | Resulting List |
|------------|----------|----------------|
| Insert 10 at beginning | Head → 10 | 10 |
| Insert 20 at end | Add after 10 | 10 20 |
| Insert 15 at pos 2 | Between 10 and 20 | 10 15 20 |
| Delete at beginning | Remove 10 | 15 20 |
| Delete at end | Remove 20 | 15 |
| Delete at pos 1 | Remove 15 | (empty) |

**Final List:** (empty)

## Conclusion
The program successfully performs all **Insertion** and **Deletion** operations on a **Doubly Linked List**, including at the beginning, end, and any specific position. It demonstrates the bidirectional traversal and linking properties of doubly linked structures.
