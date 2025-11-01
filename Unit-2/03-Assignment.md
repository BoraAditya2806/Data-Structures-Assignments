# Assignment 4

## Problem Statement
Develop a C++ program to store and manage an **appointment schedule** for a single day using a **linked list**. Appointments should be scheduled randomly. The system must define the **start time**, **end time**, and specify the **minimum and maximum duration** allowed for each appointment slot.

The program should include the following operations:  
a) Display the list of currently available time slots.  
b) Book a new appointment within the defined time limits.  
c) Cancel an existing appointment after validating its time, availability, and correctness.  
d) Sort the appointment list in order of appointment times.  
e) Sort the list based on appointment times using pointer manipulation (without swapping data values).  


## Pseudocode

1. Define a structure `Node_agb` with fields: `start_time`, `end_time`, `booked`, and `next`.  
2. Create a class `AppointmentList_agb` with functions:  
   - `createSlots_agb(start, end, minDur, maxDur)` → generate time slots between given times.  
   - `displaySlots_agb()` → show available and booked slots.  
   - `bookSlot_agb(start, end)` → mark a slot as booked.  
   - `cancelSlot_agb(start, end)` → cancel an appointment if valid.  
   - `sortByTime_agb()` → sort appointments by time using data swap.  
   - `sortByPointer_agb()` → sort using pointer manipulation without swapping data.  
3. In `main()`, take user input for start time, end time, and duration limits.  
4. Provide menu-driven options to perform all operations.  


## Code (C++)
```cpp
#include <iostream>
using namespace std;

struct Node_agb {
    int start;
    int end;
    bool booked;
    Node_agb* next;
};

class AppointmentList_agb {
    Node_agb* head;
public:
    AppointmentList_agb() { head = nullptr; }

    void createSlots_agb(int start_time, int end_time, int minDur, int maxDur) {
        int time = start_time;
        while (time + minDur <= end_time) {
            int slot_end = time + minDur;
            Node_agb* temp = new Node_agb;
            temp->start = time;
            temp->end = slot_end;
            temp->booked = false;
            temp->next = nullptr;

            if (head == nullptr)
                head = temp;
            else {
                Node_agb* p = head;
                while (p->next) p = p->next;
                p->next = temp;
            }
            time = slot_end;
        }
    }

    void displaySlots_agb() {
        Node_agb* temp = head;
        cout << "\n--- Appointment Schedule ---\n";
        while (temp) {
            cout << temp->start << ":00 - " << temp->end << ":00 -> "
                 << (temp->booked ? "Booked" : "Available") << endl;
            temp = temp->next;
        }
    }

    void bookSlot_agb(int start_t, int end_t) {
        Node_agb* temp = head;
        while (temp) {
            if (temp->start == start_t && temp->end == end_t) {
                if (!temp->booked) {
                    temp->booked = true;
                    cout << "Appointment booked successfully." << endl;
                } else
                    cout << "Slot already booked." << endl;
                return;
            }
            temp = temp->next;
        }
        cout << "Invalid slot." << endl;
    }

    void cancelSlot_agb(int start_t, int end_t) {
        Node_agb* temp = head;
        while (temp) {
            if (temp->start == start_t && temp->end == end_t) {
                if (temp->booked) {
                    temp->booked = false;
                    cout << "Appointment cancelled successfully." << endl;
                } else
                    cout << "Slot not booked." << endl;
                return;
            }
            temp = temp->next;
        }
        cout << "Invalid slot." << endl;
    }

    void sortByTime_agb() {
        if (!head) return;
        for (Node_agb* i = head; i->next; i = i->next) {
            for (Node_agb* j = i->next; j; j = j->next) {
                if (i->start > j->start) {
                    swap(i->start, j->start);
                    swap(i->end, j->end);
                    swap(i->booked, j->booked);
                }
            }
        }
    }

    void sortByPointer_agb() {
        if (!head || !head->next) return;
        bool swapped;
        do {
            swapped = false;
            Node_agb* prev = nullptr;
            Node_agb* curr = head;
            while (curr->next) {
                if (curr->start > curr->next->start) {
                    Node_agb* nxt = curr->next;
                    curr->next = nxt->next;
                    nxt->next = curr;
                    if (prev) prev->next = nxt;
                    else head = nxt;
                    swapped = true;
                    prev = nxt;
                } else {
                    prev = curr;
                    curr = curr->next;
                }
            }
        } while (swapped);
    }
};

int main() {
    AppointmentList_agb list;
    int start, end, minDur, maxDur;
    cout << "Enter start time (e.g., 9 for 9 AM): ";
    cin >> start;
    cout << "Enter end time (e.g., 17 for 5 PM): ";
    cin >> end;
    cout << "Enter minimum slot duration: ";
    cin >> minDur;
    cout << "Enter maximum slot duration: ";
    cin >> maxDur;

    list.createSlots_agb(start, end, minDur, maxDur);

    int choice, s, e;
    do {
        cout << "\n--- Appointment Menu ---\n";
        cout << "1. Display Slots\n2. Book Appointment\n3. Cancel Appointment\n4. Sort by Time\n5. Sort by Pointer\n0. Exit\nEnter choice: ";
        cin >> choice;

        switch (choice) {
        case 1:
            list.displaySlots_agb();
            break;
        case 2:
            cout << "Enter start and end time: ";
            cin >> s >> e;
            list.bookSlot_agb(s, e);
            break;
        case 3:
            cout << "Enter start and end time: ";
            cin >> s >> e;
            list.cancelSlot_agb(s, e);
            break;
        case 4:
            list.sortByTime_agb();
            cout << "Sorted using data swap." << endl;
            break;
        case 5:
            list.sortByPointer_agb();
            cout << "Sorted using pointer manipulation." << endl;
            break;
        }
    } while (choice != 0);

    return 0;
}
```

## Input/Output


```
Enter start time (e.g., 9 for 9 AM): 
9
Enter end time (e.g., 17 for 5 PM): 12
Enter minimum slot duration: 1
Enter maximum slot duration: 2

--- Appointment Menu ---
1. Display Slots
2. Book Appointment
3. Cancel Appointment
4. Sort by Time
5. Sort by Pointer
0. Exit
Enter choice: 1

--- Appointment Schedule ---
9:00 - 10:00 -> Available
10:00 - 11:00 -> Available
11:00 - 12:00 -> Available

--- Appointment Menu ---
1. Display Slots
2. Book Appointment
3. Cancel Appointment
4. Sort by Time
5. Sort by Pointer
0. Exit
Enter choice: 2
Enter start and end time: 9 10
Appointment booked successfully.

--- Appointment Menu ---
1. Display Slots
2. Book Appointment
3. Cancel Appointment
4. Sort by Time
5. Sort by Pointer
0. Exit
Enter choice: 1

--- Appointment Schedule ---
9:00 - 10:00 -> Booked
10:00 - 11:00 -> Available
11:00 - 12:00 -> Available

--- Appointment Menu ---
1. Display Slots
2. Book Appointment
3. Cancel Appointment
4. Sort by Time
5. Sort by Pointer
0. Exit
Enter choice: 3
Enter start and end time: 9 10
Appointment cancelled successfully.

--- Appointment Menu ---
1. Display Slots
2. Book Appointment
3. Cancel Appointment
4. Sort by Time
5. Sort by Pointer
0. Exit
Enter choice: 1

--- Appointment Schedule ---
9:00 - 10:00 -> Available
10:00 - 11:00 -> Available
11:00 - 12:00 -> Available

--- Appointment Menu ---
1. Display Slots
2. Book Appointment
3. Cancel Appointment
4. Sort by Time
5. Sort by Pointer
0. Exit
Enter choice: 0

```

## Dry Run 

| Step | Operation | Input | Result |
|------|------------|--------|---------|
| 1 | createSlots_agb | 9–12, 1hr | 3 slots created |
| 2 | displaySlots_agb | — | shows 3 available slots |
| 3 | bookSlot_agb | 9–10 | slot booked |
| 4 | cancelSlot_agb | 9–10 | slot freed |
| 5 | sortByPointer_agb | — | maintains time order |

**Example List Before Sorting:**  
9:00–10:00 (Booked), 11:00–12:00 (Available), 10:00–11:00 (Available)  
**After Sorting:**  
9:00–10:00, 10:00–11:00, 11:00–12:00  

## Conclusion
The **Appointment Scheduling System** efficiently manages daily appointments using a **linked list**.  
All key operations—display, booking, cancellation, and sorting—are performed dynamically with both data and pointer-based sorting approaches.
