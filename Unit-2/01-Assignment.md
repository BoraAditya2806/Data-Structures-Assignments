# Assignment 1

## Problem Statement
**Implementation of Singly Linked List to Manage ‘Vertex Club’ Membership Records.**  
The Department of Computer Engineering has a student club named ‘Vertex Club’ for second, third, and final year students. The first member is the President and the last member is the Secretary. Write a C++ program to:  
- Add/delete members (including President/Secretary)  
- Count members  
- Display members  
- Concatenate two division lists  
Also implement: reverse, search by PRN, and sort by PRN operations.


## Pseudocode

1. Define a structure `Node_agb` with fields: `PRN`, `Name`, and pointer `next`.  
2. Create a class `LinkedList_agb` containing:
   - `Node_agb* head` (points to first node).  
   - Functions for each operation:
     - `addPresident_agb()` → insert at beginning.  
     - `addSecretary_agb()` → insert at end.  
     - `addMember_agb()` → insert after a specific PRN.  
     - `deleteMember_agb()` → delete by PRN.  
     - `countMembers_agb()` → return number of nodes.  
     - `display_agb()` → print all nodes.  
     - `concatenate_agb(LinkedList_agb L2)` → join two lists.  
     - `reverse_agb()` → reverse the list.  
     - `searchByPRN_agb()` → find node by PRN.  
     - `sortByPRN_agb()` → sort the list in ascending order.  
3. In `main()`, provide a menu-driven interface to call these functions.  


## Code (C++)
```cpp
#include <iostream>
#include <string>
using namespace std;

struct Node_agb {
    int PRN;
    string name;
    Node_agb* next;
};

class LinkedList_agb {
    Node_agb* head;
public:
    LinkedList_agb() { head = nullptr; }

    Node_agb* createNode_agb(int PRN, string name) {
        Node_agb* temp = new Node_agb;
        temp->PRN = PRN;
        temp->name = name;
        temp->next = nullptr;
        return temp;
    }

    void addPresident_agb(int PRN, string name) {
        Node_agb* temp = createNode_agb(PRN, name);
        temp->next = head;
        head = temp;
    }

    void addSecretary_agb(int PRN, string name) {
        Node_agb* temp = createNode_agb(PRN, name);
        if (head == nullptr) {
            head = temp;
            return;
        }
        Node_agb* ptr = head;
        while (ptr->next != nullptr)
            ptr = ptr->next;
        ptr->next = temp;
    }

    void addMember_agb(int PRN, string name, int afterPRN) {
        Node_agb* temp = createNode_agb(PRN, name);
        Node_agb* ptr = head;
        while (ptr != nullptr && ptr->PRN != afterPRN)
            ptr = ptr->next;
        if (ptr == nullptr)
            cout << "Member with PRN " << afterPRN << " not found." << endl;
        else {
            temp->next = ptr->next;
            ptr->next = temp;
        }
    }

    void deleteMember_agb(int PRN) {
        if (head == nullptr)
            return;
        if (head->PRN == PRN) {
            Node_agb* temp = head;
            head = head->next;
            delete temp;
            return;
        }
        Node_agb* ptr = head;
        Node_agb* prev = nullptr;
        while (ptr != nullptr && ptr->PRN != PRN) {
            prev = ptr;
            ptr = ptr->next;
        }
        if (ptr == nullptr)
            return;
        prev->next = ptr->next;
        delete ptr;
    }

    void display_agb() {
        Node_agb* ptr = head;
        while (ptr != nullptr) {
            cout << ptr->PRN << " - " << ptr->name << endl;
            ptr = ptr->next;
        }
    }

    int countMembers_agb() {
        int count = 0;
        Node_agb* ptr = head;
        while (ptr != nullptr) {
            count++;
            ptr = ptr->next;
        }
        return count;
    }

    void concatenate_agb(LinkedList_agb& L2) {
        if (head == nullptr) {
            head = L2.head;
            return;
        }
        Node_agb* ptr = head;
        while (ptr->next != nullptr)
            ptr = ptr->next;
        ptr->next = L2.head;
    }

    void reverse_agb() {
        Node_agb* prev = nullptr;
        Node_agb* current = head;
        Node_agb* next = nullptr;
        while (current != nullptr) {
            next = current->next;
            current->next = prev;
            prev = current;
            current = next;
        }
        head = prev;
    }

    Node_agb* searchByPRN_agb(int PRN) {
        Node_agb* ptr = head;
        while (ptr != nullptr) {
            if (ptr->PRN == PRN)
                return ptr;
            ptr = ptr->next;
        }
        return nullptr;
    }

    void sortByPRN_agb() {
        if (head == nullptr)
            return;
        for (Node_agb* i = head; i->next != nullptr; i = i->next) {
            for (Node_agb* j = i->next; j != nullptr; j = j->next) {
                if (i->PRN > j->PRN) {
                    swap(i->PRN, j->PRN);
                    swap(i->name, j->name);
                }
            }
        }
    }
};

int main() {
    LinkedList_agb divA, divB;
    int choice, PRN, afterPRN;
    string name;

    do {
        cout << "\n--- Vertex Club Menu ---\n";
        cout << "1. Add President\n2. Add Secretary\n3. Add Member\n4. Delete Member\n";
        cout << "5. Display Members\n6. Count Members\n7. Concatenate Two Divisions\n";
        cout << "8. Reverse List\n9. Search by PRN\n10. Sort by PRN\n0. Exit\nEnter choice: ";
        cin >> choice;

        switch (choice) {
        case 1:
            cout << "Enter PRN and Name: ";
            cin >> PRN >> name;
            divA.addPresident_agb(PRN, name);
            break;
        case 2:
            cout << "Enter PRN and Name: ";
            cin >> PRN >> name;
            divA.addSecretary_agb(PRN, name);
            break;
        case 3:
            cout << "Enter PRN and Name: ";
            cin >> PRN >> name;
            cout << "Add after PRN: ";
            cin >> afterPRN;
            divA.addMember_agb(PRN, name, afterPRN);
            break;
        case 4:
            cout << "Enter PRN to delete: ";
            cin >> PRN;
            divA.deleteMember_agb(PRN);
            break;
        case 5:
            divA.display_agb();
            break;
        case 6:
            cout << "Total Members: " << divA.countMembers_agb() << endl;
            break;
        case 7:
            cout << "Enter data for Division B (3 members):\n";
            for (int i = 0; i < 3; i++) {
                cout << "Enter PRN and Name: ";
                cin >> PRN >> name;
                divB.addSecretary_agb(PRN, name);
            }
            divA.concatenate_agb(divB);
            cout << "Lists concatenated.\n";
            break;
        case 8:
            divA.reverse_agb();
            cout << "List reversed.\n";
            break;
        case 9:
            cout << "Enter PRN to search: ";
            cin >> PRN;
            if (divA.searchByPRN_agb(PRN))
                cout << "Member found.\n";
            else
                cout << "Member not found.\n";
            break;
        case 10:
            divA.sortByPRN_agb();
            cout << "List sorted by PRN.\n";
            break;
        }
    } while (choice != 0);

    return 0;
}
```

## Sample Input/Output

**Input** && **Output**
```

--- Vertex Club Menu ---
1. Add President
2. Add Secretary
3. Add Member
4. Delete Member
5. Display Members
6. Count Members
7. Concatenate Two Divisions
8. Reverse List
9. Search by PRN
10. Sort by PRN
0. Exit
Enter choice: 1
Enter PRN and Name: 101 John

--- Vertex Club Menu ---
1. Add President
2. Add Secretary
3. Add Member
4. Delete Member
5. Display Members
6. Count Members
7. Concatenate Two Divisions
8. Reverse List
9. Search by PRN
10. Sort by PRN
0. Exit
Enter choice: 2
Enter PRN and Name: 110 Sarah

--- Vertex Club Menu ---
1. Add President
2. Add Secretary
3. Add Member
4. Delete Member
5. Display Members
6. Count Members
7. Concatenate Two Divisions
8. Reverse List
9. Search by PRN
10. Sort by PRN
0. Exit
Enter choice: 3
Enter PRN and Name: 105 Mike
Add after PRN: 101

--- Vertex Club Menu ---
1. Add President
2. Add Secretary
3. Add Member
4. Delete Member
5. Display Members
6. Count Members
7. Concatenate Two Divisions
8. Reverse List
9. Search by PRN
10. Sort by PRN
0. Exit
Enter choice: 5
101 - John
105 - Mike
110 - Sarah

```

## Dry Run 

**Initial State:** Empty list  
**Operation Sequence:**
| Step | Operation | Resulting List |
|------|------------|----------------|
| 1 | addPresident_agb(101, John) | [101 - John] |
| 2 | addSecretary_agb(110, Sarah) | [101 - John → 110 - Sarah] |
| 3 | addMember_agb(105, Mike, 101) | [101 - John → 105 - Mike → 110 - Sarah] |
| 4 | reverse_agb() | [110 - Sarah → 105 - Mike → 101 - John] |
| 5 | sortByPRN_agb() | [101 - John → 105 - Mike → 110 - Sarah] |

**Final List:** [101 - John → 105 - Mike → 110 - Sarah]

## Conclusion
The program successfully manages the ‘Vertex Club’ membership records using a **Singly Linked List**.  

