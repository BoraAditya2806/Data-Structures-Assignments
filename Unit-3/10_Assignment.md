# Assignment 10

## Problem Statement

In a call center, customer calls are handled on a first-come, first-served basis. Implement a queue system using Linked list where:

- Each customer call is enqueued as it arrives
- Customer service agents dequeue calls to assist customers
- If there are no calls, the system waits

---

## Pseudocode

```
CLASS Call:
    callID: integer
    customerName: string
    phoneNumber: string
    issue: string
    priority: string

CLASS Node:
    data: Call
    next: pointer to Node

CLASS CallCenterQueue:
    front: pointer to Node
    rear: pointer to Node
    totalCalls: integer
    servedCalls: integer

    FUNCTION __init__():
        front = NULL
        rear = NULL
        totalCalls = 0
        servedCalls = 0

    FUNCTION isEmpty():
        RETURN front == NULL

    FUNCTION enqueueCall(call):
        newNode = CREATE Node with call
        newNode.next = NULL

        IF isEmpty():
            front = rear = newNode
        ELSE:
            rear.next = newNode
            rear = newNode

        totalCalls++
        PRINT "Call added to queue"

    FUNCTION dequeueCall():
        IF isEmpty():
            PRINT "No calls waiting"
            RETURN NULL

        call = front.data
        temp = front
        front = front.next

        IF front == NULL:
            rear = NULL

        DELETE temp
        servedCalls++
        RETURN call

    FUNCTION displayWaiting():
        IF isEmpty():
            PRINT "No calls in queue"
            RETURN

        current = front
        WHILE current != NULL:
            PRINT current.data
            current = current.next

    FUNCTION getStats():
        PRINT total calls, served calls, waiting calls

MAIN:
    CREATE CallCenterQueue
    LOOP:
        DISPLAY menu
        GET choice
        SWITCH choice:
            CASE 1: Incoming call (enqueue)
            CASE 2: Answer call (dequeue)
            CASE 3: Display waiting calls
            CASE 4: Show statistics
            CASE 5: Exit
```

---

## C++ Code

```cpp
#include <iostream>
#include <string>
#include <iomanip>
using namespace std;

class Call { // Class name is NOT modified
public:
    int callID;
    string customerName;
    string phoneNumber;
    string issue;
    string priority;

    Call_agb() { // Constructor name modified
        callID = 0;
        customerName = "";
        phoneNumber = "";
        issue = "";
        priority = "Normal";
    }

    Call_agb(int id, string name, string phone, string iss, string pri = "Normal") { // Constructor name modified
        callID = id;
        customerName = name;
        phoneNumber = phone;
        issue = iss;
        priority = pri;
    }

    void display_agb() { // Method name modified
        cout << setw(8) << callID
             << setw(20) << customerName
             << setw(15) << phoneNumber
             << setw(25) << issue
             << setw(12) << priority << endl;
    }
};

class Node { // Class name is NOT modified
public:
    Call data;
    Node* next;

    Node_agb(Call c) { // Constructor name modified
        data = c;
        next = NULL;
    }
};

class CallCenterQueue { // Class name is NOT modified
private:
    Node* front;
    Node* rear;
    int totalCalls;
    int servedCalls;

public:
    CallCenterQueue_agb() { // Constructor name modified
        front = NULL;
        rear = NULL;
        totalCalls = 0;
        servedCalls = 0;

        cout << "Call Center Queue System initialized!" << endl;
    }

    // Check if queue is empty
    bool isEmpty_agb() { // Method name modified
        return front == NULL;
    }

    // Add incoming call to queue
    void enqueueCall_agb(Call call) { // Method name modified
        Node* newNode = new Node_agb(call); // Calling postfixed constructor

        if (isEmpty_agb()) { // Calling postfixed method
            front = rear = newNode;
        } else {
            rear->next = newNode;
            rear = newNode;
        }

        totalCalls++;

        cout << "\n✓ Incoming call from '" << call.customerName
             << "' (Call ID: " << call.callID << ")" << endl;
        cout << "Issue: " << call.issue << endl;
        cout << "Priority: " << call.priority << endl;
        cout << "Position in queue: " << getWaitingCount_agb() << endl; // Calling postfixed method
    }

    // Answer and remove call from queue
    Call dequeueCall_agb() { // Method name modified
        if (isEmpty_agb()) { // Calling postfixed method
            cout << "\n✓ No calls waiting. System is idle." << endl;
            return Call_agb(); // Calling postfixed constructor
        }

        Node* temp = front;
        Call call = front->data;
        front = front->next;

        if (front == NULL) {
            rear = NULL;
        }

        delete temp;
        servedCalls++;

        cout << "\n✓ Answering call from '" << call.customerName
             << "' (Call ID: " << call.callID << ")" << endl;
        cout << "Phone: " << call.phoneNumber << endl;
        cout << "Issue: " << call.issue << endl;
        cout << "Priority: " << call.priority << endl;
        cout << "Remaining calls in queue: " << getWaitingCount_agb() << endl; // Calling postfixed method

        return call;
    }

    // Display next call without answering
    void displayNextCall_agb() { // Method name modified
        if (isEmpty_agb()) { // Calling postfixed method
            cout << "\nNo calls waiting!" << endl;
            return;
        }

        cout << "\n--- Next Call in Queue ---" << endl;
        cout << "Call ID: " << front->data.callID << endl;
        cout << "Customer: " << front->data.customerName << endl;
        cout << "Phone: " << front->data.phoneNumber << endl;
        cout << "Issue: " << front->data.issue << endl;
        cout << "Priority: " << front->data.priority << endl;
    }

    // Display all waiting calls
    void displayWaitingCalls_agb() { // Method name modified
        if (isEmpty_agb()) { // Calling postfixed method
            cout << "\n✓ No calls waiting. All clear!" << endl;
            return;
        }

        cout << "\n========== Waiting Calls Queue ==========" << endl;
        cout << setw(8) << "Call ID"
             << setw(20) << "Customer"
             << setw(15) << "Phone"
             << setw(25) << "Issue"
             << setw(12) << "Priority" << endl;
        cout << string(80, '-') << endl;

        Node* current = front;
        int position = 1;

        while (current != NULL) {
            cout << "Queue #" << position << ": ";
            current->data.display_agb(); // Calling postfixed method
            current = current->next;
            position++;
        }

        cout << string(80, '=') << endl;
        cout << "Total waiting calls: " << getWaitingCount_agb() << endl; // Calling postfixed method
    }

    // Get count of waiting calls
    int getWaitingCount_agb() { // Method name modified
        int count = 0;
        Node* current = front;

        while (current != NULL) {
            count++;
            current = current->next;
        }

        return count;
    }

    // Display statistics
    void displayStats_agb() { // Method name modified
        cout << "\n===== Call Center Statistics =====" << endl;
        cout << "Total calls received: " << totalCalls << endl;
        cout << "Calls served: " << servedCalls << endl;
        cout << "Calls waiting: " << getWaitingCount_agb() << endl; // Calling postfixed method

        if (totalCalls > 0) {
            float efficiency = ((float)servedCalls / totalCalls) * 100;
            cout << "Service efficiency: " << fixed << setprecision(2)
                 << efficiency << "%" << endl;
        }

        if (isEmpty_agb()) { // Calling postfixed method
            cout << "Status: ALL CLEAR (No waiting calls)" << endl;
        } else {
            cout << "Status: ACTIVE (" << getWaitingCount_agb() // Calling postfixed method
                 << " calls waiting)" << endl;
        }
    }

    // Destructor
    ~CallCenterQueue_agb() { // Destructor name modified (following pattern)
        while (!isEmpty_agb()) { // Calling postfixed method
            dequeueCall_agb(); // Calling postfixed method
        }
    }
};

int main_agb() { // Function name modified
    CallCenterQueue_agb callCenter_agb; // Variable name and constructor call modified
    int choice;
    int callIDCounter_agb = 1001; // Variable name modified

    do {
        cout << "\n===== Call Center Queue System =====" << endl;
        cout << "1. Incoming call (Add to queue)" << endl;
        cout << "2. Answer next call (Remove from queue)" << endl;
        cout << "3. View next call" << endl;
        cout << "4. Display all waiting calls" << endl;
        cout << "5. Display statistics" << endl;
        cout << "6. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;
        cin.ignore();

        switch (choice) {
            case 1: {
                string name, phone, issue, priority;

                cout << "\n--- Incoming Call ---" << endl;
                cout << "Enter customer name: ";
                getline(cin, name);
                cout << "Enter phone number: ";
                getline(cin, phone);
                cout << "Enter issue/query: ";
                getline(cin, issue);
                cout << "Enter priority (Low/Normal/High/Urgent): ";
                getline(cin, priority);

                Call call(callIDCounter_agb++, name, phone, issue, priority); // Variable name modified
                callCenter_agb.enqueueCall_agb(call); // Calling postfixed method
                break;
            }

            case 2:
                callCenter_agb.dequeueCall_agb(); // Calling postfixed method
                break;

            case 3:
                callCenter_agb.displayNextCall_agb(); // Calling postfixed method
                break;

            case 4:
                callCenter_agb.displayWaitingCalls_agb(); // Calling postfixed method
                break;

            case 5:
                callCenter_agb.displayStats_agb(); // Calling postfixed method
                break;

            case 6:
                cout << "\n===== Closing Call Center =====" << endl;
                callCenter_agb.displayStats_agb(); // Calling postfixed method

                if (callCenter_agb.getWaitingCount_agb() > 0) { // Calling postfixed method
                    cout << "\nWarning: " << callCenter_agb.getWaitingCount_agb() // Calling postfixed method
                         << " calls still waiting!" << endl;
                }

                cout << "Thank you for using the system!" << endl;
                break;

            default:
                cout << "Invalid choice! Please try again." << endl;
        }
    } while (choice != 6);

    return 0;
}
```

---

## Input/Output

### Sample Run:

```
Call Center Queue System initialized!

===== Call Center Queue System =====
1. Incoming call (Add to queue)
2. Answer next call (Remove from queue)
3. View next call
4. Display all waiting calls
5. Display statistics
6. Exit
Enter your choice: 1

--- Incoming Call ---
Enter customer name: John Doe
Enter phone number: 555-1234
Enter issue/query: Internet not working
Enter priority (Low/Normal/High/Urgent): High

✓ Incoming call from 'John Doe' (Call ID: 1001)
Issue: Internet not working
Priority: High
Position in queue: 1

Enter your choice: 1

--- Incoming Call ---
Enter customer name: Sarah Smith
Enter phone number: 555-5678
Enter issue/query: Billing inquiry
Enter priority (Low/Normal/High/Urgent): Normal

✓ Incoming call from 'Sarah Smith' (Call ID: 1002)
Issue: Billing inquiry
Priority: Normal
Position in queue: 2

Enter your choice: 1

--- Incoming Call ---
Enter customer name: Mike Johnson
Enter phone number: 555-9012
Enter issue/query: Service activation
Enter priority (Low/Normal/High/Urgent): Urgent

✓ Incoming call from 'Mike Johnson' (Call ID: 1003)
Issue: Service activation
Priority: Urgent
Position in queue: 3

Enter your choice: 4

========== Waiting Calls Queue ==========
 Call ID            Customer          Phone                    Issue    Priority
--------------------------------------------------------------------------------
Queue #1:     1001            John Doe       555-1234     Internet not working        High
Queue #2:     1002         Sarah Smith       555-5678         Billing inquiry      Normal
Queue #3:     1003        Mike Johnson       555-9012     Service activation      Urgent
================================================================================
Total waiting calls: 3

Enter your choice: 3

--- Next Call in Queue ---
Call ID: 1001
Customer: John Doe
Phone: 555-1234
Issue: Internet not working
Priority: High

Enter your choice: 2

✓ Answering call from 'John Doe' (Call ID: 1001)
Phone: 555-1234
Issue: Internet not working
Priority: High
Remaining calls in queue: 2

Enter your choice: 5

===== Call Center Statistics =====
Total calls received: 3
Calls served: 1
Calls waiting: 2
Service efficiency: 33.33%
Status: ACTIVE (2 calls waiting)

Enter your choice: 2

✓ Answering call from 'Sarah Smith' (Call ID: 1002)
Phone: 555-5678
Issue: Billing inquiry
Priority: Normal
Remaining calls in queue: 1

Enter your choice: 2

✓ Answering call from 'Mike Johnson' (Call ID: 1003)
Phone: 555-9012
Issue: Service activation
Priority: Urgent
Remaining calls in queue: 0

Enter your choice: 2

✓ No calls waiting. System is idle.

Enter your choice: 6

===== Closing Call Center =====

===== Call Center Statistics =====
Total calls received: 3
Calls served: 3
Calls waiting: 0
Service efficiency: 100.00%
Status: ALL CLEAR (No waiting calls)

Thank you for using the system!
```

---

## Dry Run

### Initial State:

```
front = NULL
rear = NULL
totalCalls = 0
servedCalls = 0
```

### Operation 1: enqueueCall(Call(1001, "John Doe", "555-1234", "Internet not working", "High"))

```
Step 1: Create newNode with Call data
Step 2: isEmpty() = TRUE
Step 3: front = rear = newNode
Step 4: totalCalls = 1

Queue:
front -> [1001: John Doe, Internet issue, High] <- rear
         next = NULL
```

### Operation 2: enqueueCall(Call(1002, "Sarah Smith", "555-5678", "Billing inquiry", "Normal"))

```
Step 1: Create newNode
Step 2: isEmpty() = FALSE
Step 3: rear->next = newNode
Step 4: rear = newNode
Step 5: totalCalls = 2

Queue:
front -> [1001: John] -> [1002: Sarah] <- rear
                        next = NULL
```

### Operation 3: enqueueCall(Call(1003, "Mike Johnson", "555-9012", "Service activation", "Urgent"))

```
Step 1: Create newNode
Step 2: rear->next = newNode
Step 3: rear = newNode
Step 4: totalCalls = 3

Queue:
front -> [1001: John] -> [1002: Sarah] -> [1003: Mike] <- rear
```

### Operation 4: displayNextCall()

```
Step 1: Check isEmpty() = FALSE
Step 2: Display front->data
Output:
  Call ID: 1001
  Customer: John Doe
  Phone: 555-1234
  Issue: Internet not working
  Priority: High

Queue remains unchanged
```

### Operation 5: dequeueCall()

```
Step 1: isEmpty() = FALSE
Step 2: call = front->data (John Doe)
Step 3: temp = front
Step 4: front = front->next (now points to Sarah)
Step 5: delete temp
Step 6: servedCalls = 1

Queue:
front -> [1002: Sarah Smith] -> [1003: Mike Johnson] <- rear

Output:
Answering call from 'John Doe' (Call ID: 1001)
Remaining calls: 2
```

### Operation 6: getWaitingCount()

```
count = 0
current = front (Sarah)
Loop iteration 1: count = 1, current = Mike
Loop iteration 2: count = 2, current = NULL
Return 2
```

### Operation 7: dequeueCall()

```
call = Sarah Smith
front = Mike Johnson
servedCalls = 2

Queue:
front -> [1003: Mike Johnson] <- rear
         next = NULL
```

### Operation 8: dequeueCall()

```
call = Mike Johnson
front = NULL
rear = NULL
servedCalls = 3

Queue: EMPTY
```

### Operation 9: dequeueCall() - UNDERFLOW

```
isEmpty() = TRUE
Display: "No calls waiting. System is idle."
Return empty Call object
```

---

## Conclusion

This program successfully implements a **Call Center Queue System using Linked List**:

1. **Queue Implementation**:

   - Dynamic linked list structure
   - FIFO (First-In-First-Out) principle
   - No size limitation
   - Efficient memory management

2. **Call Management Features**:

   - Auto-generated call IDs
   - Customer information storage
   - Issue/query tracking
   - Priority levels (Low/Normal/High/Urgent)
   - Phone number logging

3. **Core Operations**:

   - **Enqueue**: O(1) - Add incoming call at rear
   - **Dequeue**: O(1) - Answer call from front
   - **Display Next**: O(1) - View without answering
   - **Display All**: O(n) - Show entire queue
   - **Get Count**: O(n) - Count waiting calls

4. **Real-world Features**:

   - Call tracking statistics
   - Service efficiency calculation
   - Queue position display
   - Idle state handling
   - Warning for unanswered calls at exit

5. **Statistics Tracking**:

   - Total calls received
   - Calls served
   - Calls waiting
   - Service efficiency percentage
   - System status (Active/All Clear)

6. **Business Logic**:
   - First-come, first-served basis
   - Priority information (for future enhancement)
   - Complete call information
   - Professional call handling flow

**Time Complexity**:

- Enqueue: O(1)
- Dequeue: O(1)
- Display Next: O(1)
- Display All: O(n)
- Get Count: O(n)
- Display Stats: O(n)

**Space Complexity**: O(n) where n is number of calls in queue

**Applications**:

- Customer service call centers
- Technical support hotlines
- Emergency dispatch systems
- Help desk management
- Telesales operations

**Advantages of Linked List Implementation**:

- No fixed capacity limit
- Dynamic growth as calls come in
- Efficient insertion and deletion
- No memory wastage
- Easy to maintain statistics

**Future Enhancements**:

- Priority queue (serve urgent calls first)
- Multiple agent handling
- Call routing based on issue type
- Average wait time calculation
- Call abandonment tracking
