# Assignment 7

## Problem Statement
Write a C++ program to implement a **Set using a Generalized Linked List (GLL)**.  
The program should allow the user to create a nested set structure dynamically using linked lists and display it in **proper set notation format**.  

**Example:**  
S = { p, q, {r, s, t, {}, {u, v}, w, x, {y, z}, a1, b1} }


## Pseudocode

1. Define a node structure with two pointers: `down` (for sublist) and `next` (for next element).  
2. Create a recursive function to construct the GLL from user input.  
3. Each element entered can either be:  
   - An atom (simple element), or  
   - A sublist (enclosed in `{}`)  
4. Store atoms as string data and link sublists using the `down` pointer.  
5. Create a recursive display function to print the GLL in correct set format `{ ... }`.  
6. Display the final set.


## Code (C++)
```cpp
#include <iostream>
#include <string>
using namespace std;

struct Node {
    string data;
    Node *next;
    Node *down;
};

Node* create_agb() {
    Node *head = nullptr, *tail = nullptr;
    string input;
    cout << "Enter element (or '{' to start sublist, '}' to end, '0' to stop): ";
    while (cin >> input && input != "0" && input != "}") {
        Node *newNode = new Node;
        newNode->next = newNode->down = nullptr;

        if (input == "{") {
            cout << "Creating sublist..." << endl;
            newNode->down = create_agb();
            newNode->data = "{}";
        } else {
            newNode->data = input;
        }

        if (!head) head = newNode;
        else tail->next = newNode;
        tail = newNode;

        cout << "Enter next element (or '{', '}', '0'): ";
    }
    return head;
}

void display_agb(Node *head) {
    cout << "{";
    Node *temp = head;
    while (temp) {
        if (temp->down != nullptr) {
            display_agb(temp->down);
        } else if (temp->data != "{}") {
            cout << temp->data;
        }
        if (temp->next) cout << ", ";
        temp = temp->next;
    }
    cout << "}";
}

int main() {
    cout << "Create a Generalized Linked List representation of a set:\n";
    Node *set = create_agb();

    cout << "\nSet Representation in Proper Format:\n";
    display_agb(set);
    cout << endl;

    return 0;
}
```

## Input
```
Enter element (or '{' to start sublist, '}' to end, '0' to stop): p
Enter next element (or '{', '}', '0'): q
Enter next element (or '{', '}', '0'): {
Enter element (or '{' to start sublist, '}' to end, '0' to stop): r
Enter next element (or '{', '}', '0'): s
Enter next element (or '{', '}', '0'): t
Enter next element (or '{', '}', '0'): 0
Enter next element (or '{', '}', '0'): 0
```

## Output
```
Set Representation in Proper Format:
{p, q, {r, s, t}}
```

## Dry Run (Step-by-Step in Tabular Form)

| Step | Input | Action | GLL Representation |
|------|--------|----------|--------------------|
| 1 | p | Create node with data = p | {p} |
| 2 | q | Add node to next of p | {p, q} |
| 3 | { | Start sublist | {p, q, { }} |
| 4 | r | Add node in sublist | {p, q, {r}} |
| 5 | s | Add node to next of r | {p, q, {r, s}} |
| 6 | t | Add node to next of s | {p, q, {r, s, t}} |
| 7 | 0 | End sublist | {p, q, {r, s, t}} |
| 8 | 0 | End input | Final structure complete |

**Final Output:**  
`{p, q, {r, s, t}}`


## Conclusion
The program successfully implements a **Generalized Linked List (GLL)** to represent nested sets dynamically.  
