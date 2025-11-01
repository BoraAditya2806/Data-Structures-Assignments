# Assignment 2

## Problem Statement
The ticket reservation system for Galaxy Multiplex is to be implemented using a C++ program. The multiplex has 8 rows, with 8 seats in each row. A doubly circular linked list will be used to track the availability of seats in each row.Initially, assume that some seats are randomly booked. An array will store head pointers for each row’s linked list.The system should support the following operations:
a) Display the current list of available seats.
b) Book one or more seats as per customer request.
c) Cancel an existing booking when requested.	


## Pseudocode

1. Define a structure `Node_agb` with fields: `seat_no`, `status` (0 = available, 1 = booked), and pointers `next`, `prev`.  
2. Create a class `DCLL_agb` to represent a single row:  
   - `Node_agb* head` → circular linked list head node.  
   - Functions:  
     - `createSeats_agb()` → initialize 8 seats per row.  
     - `displaySeats_agb()` → display seat numbers and status.  
     - `bookSeat_agb(int seatNo)` → mark seat as booked.  
     - `cancelSeat_agb(int seatNo)` → mark seat as available.  
3. Create a class `Multiplex_agb` with an array of 8 `DCLL_agb` rows.  
4. In `main()`, implement a menu for user interaction to perform operations.  


## Code (C++)
```cpp
#include <iostream>
using namespace std;

struct Node_agb {
    int seat_no;
    int status;
    Node_agb* next;
    Node_agb* prev;
};

class DCLL_agb {
    Node_agb* head;
public:
    DCLL_agb() { head = nullptr; }

    void createSeats_agb() {
        Node_agb* last = nullptr;
        for (int i = 1; i <= 8; i++) {
            Node_agb* temp = (Node_agb*)malloc(sizeof(Node_agb*));
            if(temp== NULL)
                {
                    cout<<"Memory Allocation Failed!!"<<endl;
                    return 1;
                }
            temp->seat_no = i;
            temp->status = rand() % 2;
            temp->next = temp->prev = nullptr;

            if (head == nullptr) {
                head = temp;
                head->next = head;
                head->prev = head;
                last = head;
            } else {
                temp->prev = last;
                temp->next = head;
                last->next = temp;
                head->prev = temp;
                last = temp;
            }
        }
    }

    void displaySeats_agb() {
        if (head == nullptr)
            return;
        Node_agb* temp = head;
        do {
            cout << "[" << temp->seat_no << (temp->status ? "B" : "A") << "] ";
            temp = temp->next;
        } while (temp != head);
        cout << endl;
    }

    void bookSeat_agb(int seatNo) {
        if (head == nullptr)
            return;
        Node_agb* temp = head;
        do {
            if (temp->seat_no == seatNo) {
                if (temp->status == 0) {
                    temp->status = 1;
                    cout << "Seat " << seatNo << " booked successfully." << endl;
                } else
                    cout << "Seat already booked." << endl;
                return;
            }
            temp = temp->next;
        } while (temp != head);
    }

    void cancelSeat_agb(int seatNo) {
        if (head == nullptr)
            return;
        Node_agb* temp = head;
        do {
            if (temp->seat_no == seatNo) {
                if (temp->status == 1) {
                    temp->status = 0;
                    cout << "Booking cancelled for seat " << seatNo << "." << endl;
                } else
                    cout << "Seat not booked." << endl;
                return;
            }
            temp = temp->next;
        } while (temp != head);
    }
};

class Multiplex_agb {
    DCLL_agb rows[8];
public:
    void initialize_agb() {
        for (int i = 0; i < 8; i++)
            rows[i].createSeats_agb();
    }

    void displayAll_agb() {
        for (int i = 0; i < 8; i++) {
            cout << "Row " << i + 1 << ": ";
            rows[i].displaySeats_agb();
        }
    }

    void bookSeat_agb(int rowNo, int seatNo) {
        if (rowNo < 1 || rowNo > 8) return;
        rows[rowNo - 1].bookSeat_agb(seatNo);
    }

    void cancelSeat_agb(int rowNo, int seatNo) {
        if (rowNo < 1 || rowNo > 8) return;
        rows[rowNo - 1].cancelSeat_agb(seatNo);
    }
};

int main() {
    Multiplex_agb m;
    m.initialize_agb();

    int choice, row, seat;
    do {
        cout << "\n--- Galaxy Multiplex Ticket System ---\n";
        cout << "1. Display Available Seats\n2. Book Seat\n3. Cancel Booking\n0. Exit\nEnter choice: ";
        cin >> choice;

        switch (choice) {
        case 1:
            m.displayAll_agb();
            break;
        case 2:
            cout << "Enter Row (1-8) and Seat No (1-8): ";
            cin >> row >> seat;
            m.bookSeat_agb(row, seat);
            break;
        case 3:
            cout << "Enter Row (1-8) and Seat No (1-8): ";
            cin >> row >> seat;
            m.cancelSeat_agb(row, seat);
            break;
        }
    } while (choice != 0);

    return 0;
}
```

##  Input/Output


```
--- Galaxy Multiplex Ticket System ---
1. Display Available Seats
2. Book Seat
3. Cancel Booking
0. Exit
Enter choice: 1
Row 1: [1B] [2A] [3B] [4B] [5B] [6B] [7A] [8A] 
Row 2: [1B] [2B] [3A] [4B] [5A] [6B] [7B] [8A] 
Row 3: [1A] [2A] [3A] [4A] [5B] [6A] [7B] [8B] 
Row 4: [1A] [2A] [3A] [4B] [5B] [6B] [7B] [8A] 
Row 5: [1A] [2A] [3B] [4B] [5B] [6A] [7B] [8A] 
Row 6: [1B] [2B] [3B] [4B] [5A] [6B] [7A] [8A] 
Row 7: [1B] [2A] [3B] [4A] [5B] [6A] [7A] [8B] 
Row 8: [1A] [2A] [3A] [4B] [5B] [6B] [7A] [8B] 

--- Galaxy Multiplex Ticket System ---
1. Display Available Seats
2. Book Seat
3. Cancel Booking
0. Exit
Enter choice: 2
Enter Row (1-8) and Seat No (1-8): 3 5
Seat already booked.

--- Galaxy Multiplex Ticket System ---
1. Display Available Seats
2. Book Seat
3. Cancel Booking
0. Exit
Enter choice: 3
Enter Row (1-8) and Seat No (1-8): 3 5
Booking cancelled for seat 5.

```

## Dry Run (Step-by-Step Example)

**Initial Setup:** Each row created with random seat statuses (A = Available, B = Booked)  
| Step | Operation | Affected Row | Result |
|------|------------|---------------|--------|
| 1 | displayAll_agb() | All | Shows current seat availability |
| 2 | bookSeat_agb(3, 5) | Row 3 | Seat 5 set to booked |
| 3 | cancelSeat_agb(3, 5) | Row 3 | Seat 5 set to available again |

**Example Row:** `[1A] [2B] [3A] [4B] [5A] [6A] [7B] [8A]`  
After booking seat 5 → `[1A] [2B] [3A] [4B] [5B] [6A] [7B] [8A]`  
After cancelling seat 5 → `[1A] [2B] [3A] [4B] [5A] [6A] [7B] [8A]`  

## Conclusion
The **Galaxy Multiplex Ticket Reservation System** is implemented using a **Doubly Circular Linked List** to manage seat availability.  
All operations—displaying, booking, and cancelling—are efficiently handled with pointer-based seat management across multiple rows.
