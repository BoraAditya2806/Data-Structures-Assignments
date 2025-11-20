# Assignment 8
## Problem Statement

Write a program that maintains a queue of passengers waiting to see a ticket agent. The program user should be able to insert a new passenger at the rear of the queue, display the passenger at the front of the queue, or remove the passenger at the front of the queue. The program will display the number of passengers left in the queue just before it terminates.

---

## Pseudocode

```
CLASS Passenger:
    name: string
    ticketNumber: integer
    destination: string

CLASS Node:
    data: Passenger
    next: pointer to Node

CLASS PassengerQueue:
    front: pointer to Node
    rear: pointer to Node
    count: integer

    FUNCTION __init__():
        front = NULL
        rear = NULL
        count = 0

    FUNCTION isEmpty():
        RETURN front == NULL

    FUNCTION enqueue(passenger):
        newNode = CREATE Node with passenger
        newNode.next = NULL

        IF isEmpty():
            front = rear = newNode
        ELSE:
            rear.next = newNode
            rear = newNode

        count++
        PRINT "Passenger added to queue"

    FUNCTION dequeue():
        IF isEmpty():
            PRINT "Queue is empty"
            RETURN NULL

        passenger = front.data
        temp = front
        front = front.next

        IF front == NULL:
            rear = NULL

        DELETE temp
        count--
        RETURN passenger

    FUNCTION displayFront():
        IF isEmpty():
            PRINT "No passengers in queue"
            RETURN

        PRINT front.data

    FUNCTION getCount():
        RETURN count

    FUNCTION displayAll():
        IF isEmpty():
            PRINT "Queue is empty"
            RETURN

        current = front
        position = 1
        WHILE current != NULL:
            PRINT position, current.data
            current = current.next
            position++

MAIN:
    CREATE PassengerQueue object
    LOOP:
        DISPLAY menu
        GET choice
        SWITCH choice:
            CASE 1: Insert passenger (enqueue)
            CASE 2: Display front passenger
            CASE 3: Remove front passenger (dequeue)
            CASE 4: Display all passengers
            CASE 5: Exit and show count
```

---

## C++ Code

```cpp
#include <iostream>
#include <string>
#include <iomanip>
using namespace std;

class Passenger {
public:
    string name;
    int ticketNumber;
    string destination;
    string seatClass;

    Passenger_agb() {
        name = "";
        ticketNumber = 0;
        destination = "";
        seatClass = "Economy";
    }

    Passenger_agb(string n, int tNum, string dest, string sClass = "Economy") {
        name = n;
        ticketNumber = tNum;
        destination = dest;
        seatClass = sClass;
    }

    void display_agb() {
        cout << setw(12) << ticketNumber
             << setw(20) << name
             << setw(20) << destination
             << setw(15) << seatClass << endl;
    }
};

class Node {
public:
    Passenger data;
    Node* next;

    Node_agb(Passenger p) {
        data = p;
        next = NULL;
    }
};

class PassengerQueue {
private:
    Node* front;
    Node* rear;
    int count;

public:
    PassengerQueue_agb() {
        front = NULL;
        rear = NULL;
        count = 0;
    }

    bool isEmpty_agb() {
        return front == NULL;
    }

    void enqueue_agb(Passenger passenger) {
        Node* newNode = new Node_agb(passenger);

        if (isEmpty_agb()) {
            front = rear = newNode;
        } else {
            rear->next = newNode;
            rear = newNode;
        }

        count++;
        cout << "\n✓ Passenger '" << passenger.name << "' (Ticket #"
             << passenger.ticketNumber << ") added to queue." << endl;
        cout << "Queue position: " << count << endl;
    }

    Passenger dequeue_agb() {
        if (isEmpty_agb()) {
            cout << "Queue is empty! No passengers waiting." << endl;
            return Passenger_agb();
        }

        Node* temp = front;
        Passenger passenger = front->data;
        front = front->next;

        if (front == NULL) {
            rear = NULL;
        }

        delete temp;
        count--;

        cout << "\n✓ Passenger '" << passenger.name << "' (Ticket #"
             << passenger.ticketNumber << ") served." << endl;
        cout << "Remaining passengers: " << count << endl;

        return passenger;
    }

    void displayFront_agb() {
        if (isEmpty_agb()) {
            cout << "\nNo passengers in queue!" << endl;
            return;
        }

        cout << "\n--- Next Passenger in Line ---" << endl;
        cout << "Ticket Number: " << front->data.ticketNumber << endl;
        cout << "Name: " << front->data.name << endl;
        cout << "Destination: " << front->data.destination << endl;
        cout << "Class: " << front->data.seatClass << endl;
    }

    void displayAll_agb() {
        if (isEmpty_agb()) {
            cout << "\nQueue is empty! No passengers waiting." << endl;
            return;
        }

        cout << "\n========== Passenger Queue ==========" << endl;
        cout << setw(12) << "Ticket#"
             << setw(20) << "Name"
             << setw(20) << "Destination"
             << setw(15) << "Class" << endl;
        cout << string(67, '-') << endl;

        Node* current = front;
        int position = 1;

        while (current != NULL) {
            cout << "Position " << position << ": ";
            current->data.display_agb();
            current = current->next;
            position++;
        }

        cout << string(67, '=') << endl;
        cout << "Total passengers waiting: " << count << endl;
    }

    int getCount_agb() {
        return count;
    }

    ~PassengerQueue_agb() {
        while (!isEmpty_agb()) {
            Node* temp = front;
            front = front->next;
            delete temp;
        }
        rear = NULL;
    }
};

int main_agb() {
    PassengerQueue ticketQueue_agb;
    int choice;
    int ticketCounter_agb = 1001;

    do {
        cout << "\n===== Ticket Agent - Passenger Queue System =====" << endl;
        cout << "1. Add new passenger to queue" << endl;
        cout << "2. Display next passenger" << endl;
        cout << "3. Serve next passenger (remove from queue)" << endl;
        cout << "4. Display all waiting passengers" << endl;
        cout << "5. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;
        cin.ignore();

        switch (choice) {
            case 1: {
                string name, destination, seatClass;

                cout << "\n--- Add New Passenger ---" << endl;
                cout << "Enter passenger name: ";
                getline(cin, name);
                cout << "Enter destination: ";
                getline(cin, destination);
                cout << "Enter class (Economy/Business/First): ";
                getline(cin, seatClass);

                Passenger p(name, ticketCounter_agb++, destination, seatClass);
                ticketQueue_agb.enqueue_agb(p);
                break;
            }

            case 2:
                ticketQueue_agb.displayFront_agb();
                break;

            case 3:
                ticketQueue_agb.dequeue_agb();
                break;

            case 4:
                ticketQueue_agb.displayAll_agb();
                break;

            case 5:
                cout << "\n===== System Terminating =====" << endl;
                cout << "Total passengers left in queue: "
                     << ticketQueue_agb.getCount_agb() << endl;

                if (ticketQueue_agb.getCount_agb() > 0) {
                    cout << "\nRemaining passengers:" << endl;
                    ticketQueue_agb.displayAll_agb();
                }

                cout << "\nThank you for using the system!" << endl;
                break;

            default:
                cout << "Invalid choice! Please try again." << endl;
        }
    } while (choice != 5);

    return 0;
}
```

---

## Input/Output

### Sample Run:

```
===== Ticket Agent - Passenger Queue System =====
1. Add new passenger to queue
2. Display next passenger
3. Serve next passenger (remove from queue)
4. Display all waiting passengers
5. Exit
Enter your choice: 1

--- Add New Passenger ---
Enter passenger name: John Smith
Enter destination: New York
Enter class (Economy/Business/First): Business

✓ Passenger 'John Smith' (Ticket #1001) added to queue.
Queue position: 1

Enter your choice: 1

--- Add New Passenger ---
Enter passenger name: Sarah Connor
Enter destination: Los Angeles
Enter class (Economy/Business/First): Economy

✓ Passenger 'Sarah Connor' (Ticket #1002) added to queue.
Queue position: 2

Enter your choice: 1

--- Add New Passenger ---
Enter passenger name: Mike Johnson
Enter destination: Chicago
Enter class (Economy/Business/First): First

✓ Passenger 'Mike Johnson' (Ticket #1003) added to queue.
Queue position: 3

Enter your choice: 4

========== Passenger Queue ==========
     Ticket#                Name         Destination          Class
-------------------------------------------------------------------
Position 1:         1001          John Smith            New York       Business
Position 2:         1002       Sarah Connor         Los Angeles        Economy
Position 3:         1003       Mike Johnson             Chicago          First
===================================================================
Total passengers waiting: 3

Enter your choice: 2

--- Next Passenger in Line ---
Ticket Number: 1001
Name: John Smith
Destination: New York
Class: Business

Enter your choice: 3

✓ Passenger 'John Smith' (Ticket #1001) served.
Remaining passengers: 2

Enter your choice: 4

========== Passenger Queue ==========
     Ticket#                Name         Destination          Class
-------------------------------------------------------------------
Position 1:         1002       Sarah Connor         Los Angeles        Economy
Position 2:         1003       Mike Johnson             Chicago          First
===================================================================
Total passengers waiting: 2

Enter your choice: 3

✓ Passenger 'Sarah Connor' (Ticket #1002) served.
Remaining passengers: 1

Enter your choice: 1

--- Add New Passenger ---
Enter passenger name: Emma Watson
Enter destination: Boston
Enter class (Economy/Business/First): Business

✓ Passenger 'Emma Watson' (Ticket #1004) added to queue.
Queue position: 2

Enter your choice: 5

===== System Terminating =====
Total passengers left in queue: 2

Remaining passengers:

========== Passenger Queue ==========
     Ticket#                Name         Destination          Class
-------------------------------------------------------------------
Position 1:         1003       Mike Johnson             Chicago          First
Position 2:         1004        Emma Watson              Boston       Business
===================================================================
Total passengers waiting: 2

Thank you for using the system!
```

---

## Dry Run

### Initial State:

```
front = NULL
rear = NULL
count = 0
```

### Operation 1: enqueue(Passenger("John Smith", 1001, "New York", "Business"))

```
Step 1: Create newNode with Passenger data
Step 2: isEmpty() = TRUE
Step 3: front = rear = newNode
Step 4: count = 1

Queue:
front -> [1001: John Smith, New York, Business] <- rear
         next = NULL
```

### Operation 2: enqueue(Passenger("Sarah Connor", 1002, "Los Angeles", "Economy"))

```
Step 1: Create newNode
Step 2: isEmpty() = FALSE
Step 3: rear->next = newNode
Step 4: rear = newNode
Step 5: count = 2

Queue:
front -> [1001: John] -> [1002: Sarah] <- rear
                        next = NULL
```

### Operation 3: enqueue(Passenger("Mike Johnson", 1003, "Chicago", "First"))

```
Step 1: Create newNode
Step 2: rear->next = newNode
Step 3: rear = newNode
Step 4: count = 3

Queue:
front -> [1001: John] -> [1002: Sarah] -> [1003: Mike] <- rear
```

### Operation 4: displayFront()

```
Step 1: Check isEmpty() = FALSE
Step 2: Display front->data
Output:
  Ticket Number: 1001
  Name: John Smith
  Destination: New York
  Class: Business

Queue remains unchanged
```

### Operation 5: dequeue()

```
Step 1: isEmpty() = FALSE
Step 2: passenger = front->data (John Smith)
Step 3: temp = front
Step 4: front = front->next (now points to Sarah)
Step 5: delete temp
Step 6: count = 2

Queue:
front -> [1002: Sarah Connor] -> [1003: Mike Johnson] <- rear
```

### Operation 6: dequeue()

```
Step 1: passenger = front->data (Sarah Connor)
Step 2: temp = front
Step 3: front = front->next (now points to Mike)
Step 4: delete temp
Step 5: count = 1

Queue:
front -> [1003: Mike Johnson] <- rear
         next = NULL
```

### Operation 7: enqueue(Passenger("Emma Watson", 1004, "Boston", "Business"))

```
Step 1: Create newNode
Step 2: rear->next = newNode
Step 3: rear = newNode
Step 4: count = 2

Queue:
front -> [1003: Mike] -> [1004: Emma] <- rear
```

---

## Conclusion

This program successfully implements a **Passenger Queue System for Ticket Agent** with the following achievements:

1. **Queue Implementation Using Linked List**:

   - Dynamic size allocation
   - No fixed capacity limit
   - Efficient memory utilization
   - FIFO (First-In-First-Out) principle

2. **Core Operations**:

   - **Enqueue**: O(1) - Add passenger at rear
   - **Dequeue**: O(1) - Remove passenger from front
   - **Display Front**: O(1) - View next passenger
   - **Display All**: O(n) - Show entire queue
   - **Get Count**: O(1) - Track queue size

3. **Passenger Information Management**:

   - Ticket number (auto-generated)
   - Passenger name
   - Destination
   - Seat class (Economy/Business/First)

4. **Real-world Features**:

   - Automatic ticket numbering
   - Position tracking in queue
   - Fair serving order (FIFO)
   - Exit summary showing remaining passengers

5. **Queue Properties Maintained**:

   - Front pointer: Points to first passenger (next to be served)
   - Rear pointer: Points to last passenger (most recently added)
   - Count: Tracks total passengers efficiently

6. **User-Friendly Interface**:
   - Menu-driven system
   - Clear prompts and confirmations
   - Formatted output for readability
   - Exit summary with remaining count

**Time Complexity**:

- Enqueue: O(1)
- Dequeue: O(1)
- Display Front: O(1)
- Display All: O(n)
- Get Count: O(1)

**Space Complexity**: O(n) where n is number of passengers

**Applications**:

- Airline ticket counters
- Train reservation systems
- Bus booking offices
- Customer service centers
- Government service counters

**Advantages of Linked List Implementation**:

- No size limitation
- Dynamic growth
- Efficient insertion/deletion
- No memory wastage
- Easy to maintain front and rear pointers
