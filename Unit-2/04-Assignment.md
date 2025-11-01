# Assignment 5

## Problem Statement
In the Second Year Computer Engineering class, there are two groups of students based on their favorite sports:
- Set A includes students who like Cricket.
- Set B includes students who like Football.

Write a C++ program to represent these two sets using **linked lists** and perform the following operations:
a) Find and display the set of students who like **both Cricket and Football**.  
b) Find and display the set of students who like **either Cricket or Football, but not both**.  
c) Display the number of students who like **neither Cricket nor Football**.

The total number of students is provided by the user, and each student is represented by a unique Roll Number (ID).


## Pseudocode

1. Input total number of students `n`.  
2. Input roll numbers for students who like Cricket (Set A).  
3. Input roll numbers for students who like Football (Set B).  
4. Represent both sets using linked lists.  
5. Perform the following operations:  
   - **Intersection:** Students who like both Cricket and Football.  
   - **Symmetric Difference:** Students who like either Cricket or Football but not both.  
   - **Complement:** Students who like neither Cricket nor Football.  
6. Display the results accordingly.


## Code (C++)
```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node *next;
};

Node* insert_agb(Node *head, int val) {
    Node *newNode = new Node{val, nullptr};
    if (!head) return newNode;
    Node *temp = head;
    while (temp->next) temp = temp->next;
    temp->next = newNode;
    return head;
}

bool search_agb(Node *head, int val) {
    while (head) {
        if (head->data == val) return true;
        head = head->next;
    }
    return false;
}

void display_agb(Node *head) {
    while (head) {
        cout << head->data << " ";
        head = head->next;
    }
    cout << endl;
}

Node* intersection_agb(Node *a, Node *b) {
    Node *result = nullptr;
    for (Node *p = a; p != nullptr; p = p->next) {
        if (search_agb(b, p->data)) {
            result = insert_agb(result, p->data);
        }
    }
    return result;
}

Node* symmetricDiff_agb(Node *a, Node *b) {
    Node *result = nullptr;
    for (Node *p = a; p != nullptr; p = p->next) {
        if (!search_agb(b, p->data)) result = insert_agb(result, p->data);
    }
    for (Node *q = b; q != nullptr; q = q->next) {
        if (!search_agb(a, q->data)) result = insert_agb(result, q->data);
    }
    return result;
}

int count_agb(Node *head) {
    int c = 0;
    while (head) {
        c++;
        head = head->next;
    }
    return c;
}

int main() {
    int nA, nB, total;
    Node *cricket = nullptr, *football = nullptr;

    cout << "Enter total number of students: ";
    cin >> total;

    cout << "Enter number of students who like Cricket: ";
    cin >> nA;
    cout << "Enter roll numbers: ";
    for (int i = 0; i < nA; i++) {
        int x; cin >> x;
        cricket = insert_agb(cricket, x);
    }

    cout << "Enter number of students who like Football: ";
    cin >> nB;
    cout << "Enter roll numbers: ";
    for (int i = 0; i < nB; i++) {
        int x; cin >> x;
        football = insert_agb(football, x);
    }

    cout << "\nSet A (Cricket): ";
    display_agb(cricket);
    cout << "Set B (Football): ";
    display_agb(football);

    Node *both = intersection_agb(cricket, football);
    cout << "\nStudents who like both Cricket and Football: ";
    display_agb(both);

    Node *either = symmetricDiff_agb(cricket, football);
    cout << "Students who like either Cricket or Football but not both: ";
    display_agb(either);

    int neither = total - (count_agb(both) + count_agb(either));
    cout << "Number of students who like neither Cricket nor Football: " << neither << endl;

    return 0;
}
```

## Input && Output
```
Enter total number of students: 10
Enter number of students who like Cricket: 4
Enter roll numbers: 1 2 3 4
Enter number of students who like Football: 5
Enter roll numbers: 3 4 5 6 7

Set A (Cricket): 1 2 3 4 
Set B (Football): 3 4 5 6 7 

Students who like both Cricket and Football: 3 4 
Students who like either Cricket or Football but not both: 1 2 5 6 7 
Number of students who like neither Cricket nor Football: 3

```

## Dry Run 

| Step | Operation | Result |
|------|------------|---------|
| 1 | Set A = [1, 2, 3, 4] | Students who like Cricket |
| 2 | Set B = [3, 4, 5, 6, 7] | Students who like Football |
| 3 | Intersection (A ∩ B) = [3, 4] | Both Cricket and Football |
| 4 | Symmetric Difference (A Δ B) = [1, 2, 5, 6, 7] | Either but not both |
| 5 | Neither = Total -(A ∩ B + A Δ B) = 10 - (2 + 5) = 3 | Neither sport |

**Final Results:**  
- Both: [3, 4]  
- Either but not both: [1, 2, 5, 6, 7]  
- Neither: 3 students  

## Conclusion
The program successfully implements **set operations using linked lists** to manage student preferences for Cricket and Football. It accurately determines students who like both, either, or neither sport through linked list operations.
