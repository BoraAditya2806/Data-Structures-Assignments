# Assignment 6

## Problem Statement

Write a program to keep track of patients as they checked into a medical clinic, assigning patients to doctors on a first-come, first-served basis.

The system should support:

- Adding new patients to the queue
- Assigning the next patient to a doctor (removing from queue)
- Displaying all waiting patients
- Checking queue status

---

## Pseudocode

```
CLASS Patient:
    name: string
    patientID: integer
    ailment: string

CLASS Node:
    data: Patient
    next: pointer to Node

CLASS PatientQueue:
    front: pointer to Node
    rear: pointer to Node

    FUNCTION __init__():
        front = NULL
        rear = NULL

    FUNCTION isEmpty():
        RETURN front == NULL

    FUNCTION enqueue(patient):
        newNode = CREATE Node with patient data
        newNode.next = NULL

        IF isEmpty():
            front = rear = newNode
        ELSE:
            rear.next = newNode
            rear = newNode

        PRINT "Patient added to queue"

    FUNCTION dequeue():
        IF isEmpty():
            PRINT "Queue is empty"
            RETURN NULL

        patient = front.data
        temp = front
        front = front.next

        IF front == NULL:
            rear = NULL

        DELETE temp
        RETURN patient

    FUNCTION display():
        IF isEmpty():
            PRINT "No patients waiting"
            RETURN

        current = front
        WHILE current != NULL:
            PRINT current.data
            current = current.next

    FUNCTION getCount():
        count = 0
        current = front
        WHILE current != NULL:
            count++
            current = current.next
        RETURN count

MAIN:
    CREATE PatientQueue object
    LOOP:
        DISPLAY menu
        GET choice
        SWITCH choice:
            CASE 1: Add patient
            CASE 2: Assign to doctor (dequeue)
            CASE 3: Display all patients
            CASE 4: Get waiting count
            CASE 5: Exit
```

---

## C++ Code

```cpp
#include <iostream>
#include <string>
#include <iomanip>
using namespace std;

class Patient {
public:
    string name;
    int patientID;
    string ailment;
    int age;
    string priority;

    Patient_agb() {
        name = "";
        patientID = 0;
        ailment = "";
        age = 0;
        priority = "Normal";
    }

    Patient_agb(string n, int id, string a, int ag, string p = "Normal") {
        name = n;
        patientID = id;
        ailment = a;
        age = ag;
        priority = p;
    }

    void display_agb() {
        cout << setw(10) << patientID
             << setw(20) << name
             << setw(6) << age
             << setw(20) << ailment
             << setw(12) << priority << endl;
    }
};

class Node {
public:
    Patient data;
    Node* next;

    Node_agb(Patient p) {
        data = p;
        next = NULL;
    }
};

class PatientQueue {
private:
    Node* front;
    Node* rear;
    int count;

public:
    PatientQueue_agb() {
        front = NULL;
        rear = NULL;
        count = 0;
    }

    bool isEmpty_agb() {
        return front == NULL;
    }

    void enqueue_agb(Patient patient) {
        Node* newNode = new Node_agb(patient);

        if (isEmpty_agb()) {
            front = rear = newNode;
        } else {
            rear->next = newNode;
            rear = newNode;
        }

        count++;
        cout << "\n✓ Patient '" << patient.name << "' (ID: " << patient.patientID
             << ") added to queue." << endl;
        cout << "Position in queue: " << count << endl;
    }

    Patient dequeue_agb() {
        if (isEmpty_agb()) {
            cout << "Queue is empty! No patients waiting." << endl;
            return Patient_agb();
        }

        Node* temp = front;
        Patient patient = front->data;
        front = front->next;

        if (front == NULL) {
            rear = NULL;
        }

        delete temp;
        count--;

        cout << "\n✓ Patient '" << patient.name << "' (ID: " << patient.patientID
             << ") assigned to doctor." << endl;
        return patient;
    }

    Patient peek_agb() {
        if (isEmpty_agb()) {
            cout << "No patients in queue!" << endl;
            return Patient_agb();
        }
        return front->data;
    }

    void display_agb() {
        if (isEmpty_agb()) {
            cout << "\nNo patients waiting in queue!" << endl;
            return;
        }

        cout << "\n========== Waiting Patients Queue ==========" << endl;
        cout << setw(10) << "ID"
             << setw(20) << "Name"
             << setw(6) << "Age"
             << setw(20) << "Ailment"
             << setw(12) << "Priority" << endl;
        cout << string(68, '-') << endl;

        Node* current = front;
        int position = 1;
        while (current != NULL) {
            cout << "Queue #" << position++ << ": ";
            current->data.display_agb();
            current = current->next;
        }
        cout << string(68, '=') << endl;
        cout << "Total patients waiting: " << count << endl;
    }

    int getCount_agb() {
        return count;
    }

    ~PatientQueue_agb() {
        while (!isEmpty_agb()) {
            Node* temp = front;
            front = front->next;
            delete temp;
        }
        rear = NULL;
    }
};

int main_agb() {
    PatientQueue clinic_agb;
    int choice;

    do {
        cout << "\n===== Medical Clinic Patient Queue System =====" << endl;
        cout << "1. Check-in new patient" << endl;
        cout << "2. Assign next patient to doctor" << endl;
        cout << "3. View next patient" << endl;
        cout << "4. Display all waiting patients" << endl;
        cout << "5. Get waiting count" << endl;
        cout << "6. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;
        cin.ignore();

        switch (choice) {
            case 1: {
                string name, ailment, priority;
                int id, age;

                cout << "\n--- Patient Check-in ---" << endl;
                cout << "Enter Patient ID: ";
                cin >> id;
                cin.ignore();
                cout << "Enter Patient Name: ";
                getline(cin, name);
                cout << "Enter Age: ";
                cin >> age;
                cin.ignore();
                cout << "Enter Ailment: ";
                getline(cin, ailment);
                cout << "Enter Priority (Normal/Urgent): ";
                getline(cin, priority);

                Patient p(name, id, ailment, age, priority);
                clinic_agb.enqueue_agb(p);
                break;
            }

            case 2: {
                Patient p = clinic_agb.dequeue_agb();
                if (p.patientID != 0) {
                    cout << "Patients remaining in queue: " << clinic_agb.getCount_agb() << endl;
                }
                break;
            }

            case 3: {
                Patient p = clinic_agb.peek_agb();
                if (p.patientID != 0) {
                    cout << "\n--- Next Patient ---" << endl;
                    cout << "ID: " << p.patientID << endl;
                    cout << "Name: " << p.name << endl;
                    cout << "Age: " << p.age << endl;
                    cout << "Ailment: " << p.ailment << endl;
                    cout << "Priority: " << p.priority << endl;
                }
                break;
            }

            case 4:
                clinic_agb.display_agb();
                break;

            case 5:
                cout << "\nTotal patients waiting: " << clinic_agb.getCount_agb() << endl;
                break;

            case 6:
                cout << "Exiting system... Thank you!" << endl;
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
===== Medical Clinic Patient Queue System =====
1. Check-in new patient
2. Assign next patient to doctor
3. View next patient
4. Display all waiting patients
5. Get waiting count
6. Exit
Enter your choice: 1

--- Patient Check-in ---
Enter Patient ID: 101
Enter Patient Name: John Smith
Enter Age: 45
Enter Ailment: Fever
Enter Priority (Normal/Urgent): Normal

✓ Patient 'John Smith' (ID: 101) added to queue.
Position in queue: 1

Enter your choice: 1

--- Patient Check-in ---
Enter Patient ID: 102
Enter Patient Name: Sarah Johnson
Enter Age: 32
Enter Ailment: Headache
Enter Priority (Normal/Urgent): Normal

✓ Patient 'Sarah Johnson' (ID: 102) added to queue.
Position in queue: 2

Enter your choice: 1

--- Patient Check-in ---
Enter Patient ID: 103
Enter Patient Name: Mike Davis
Enter Age: 28
Enter Ailment: Chest Pain
Enter Priority (Normal/Urgent): Urgent

✓ Patient 'Mike Davis' (ID: 103) added to queue.
Position in queue: 3

Enter your choice: 4

========== Waiting Patients Queue ==========
        ID                Name   Age             Ailment    Priority
--------------------------------------------------------------------
Queue #1:       101          John Smith    45               Fever      Normal
Queue #2:       102      Sarah Johnson    32            Headache      Normal
Queue #3:       103          Mike Davis    28         Chest Pain      Urgent
====================================================================
Total patients waiting: 3

Enter your choice: 3

--- Next Patient ---
ID: 101
Name: John Smith
Age: 45
Ailment: Fever
Priority: Normal

Enter your choice: 2

✓ Patient 'John Smith' (ID: 101) assigned to doctor.
Patients remaining in queue: 2

Enter your choice: 4

========== Waiting Patients Queue ==========
        ID                Name   Age             Ailment    Priority
--------------------------------------------------------------------
Queue #1:       102      Sarah Johnson    32            Headache      Normal
Queue #2:       103          Mike Davis    28         Chest Pain      Urgent
====================================================================
Total patients waiting: 2

Enter your choice: 2

✓ Patient 'Sarah Johnson' (ID: 102) assigned to doctor.
Patients remaining in queue: 1

Enter your choice: 5

Total patients waiting: 1

Enter your choice: 6
Exiting system... Thank you!
```

---

## Dry Run

### Initial State:

```
front = NULL
rear = NULL
count = 0
```

### Operation 1: enqueue(Patient("John Smith", 101, "Fever", 45, "Normal"))

```
Step 1: Create newNode with Patient data
Step 2: isEmpty() = TRUE
Step 3: front = rear = newNode
Step 4: count = 1

Queue Structure:
front -> [101: John Smith] <- rear
         next = NULL
```

### Operation 2: enqueue(Patient("Sarah Johnson", 102, "Headache", 32, "Normal"))

```
Step 1: Create newNode with Patient data
Step 2: isEmpty() = FALSE
Step 3: rear->next = newNode
Step 4: rear = newNode
Step 5: count = 2

Queue Structure:
front -> [101: John Smith] -> [102: Sarah Johnson] <- rear
         next ─────────────>   next = NULL
```

### Operation 3: enqueue(Patient("Mike Davis", 103, "Chest Pain", 28, "Urgent"))

```
Step 1: Create newNode
Step 2: rear->next = newNode
Step 3: rear = newNode
Step 4: count = 3

Queue Structure:
front -> [101: John] -> [102: Sarah] -> [103: Mike] <- rear
```

### Operation 4: peek()

```
Step 1: Check isEmpty() = FALSE
Step 2: Return front->data (John Smith)

Queue remains unchanged
```

### Operation 5: dequeue()

```
Step 1: Check isEmpty() = FALSE
Step 2: patient = front->data (John Smith)
Step 3: temp = front
Step 4: front = front->next (now points to Sarah)
Step 5: delete temp
Step 6: count = 2

Queue Structure:
front -> [102: Sarah Johnson] -> [103: Mike Davis] <- rear
```

### Operation 6: dequeue()

```
Step 1: patient = front->data (Sarah Johnson)
Step 2: temp = front
Step 3: front = front->next (now points to Mike)
Step 4: delete temp
Step 5: count = 1

Queue Structure:
front -> [103: Mike Davis] <- rear
         next = NULL
```

### Operation 7: dequeue()

```
Step 1: patient = front->data (Mike Davis)
Step 2: temp = front
Step 3: front = front->next (NULL)
Step 4: rear = NULL (queue empty)
Step 5: delete temp
Step 6: count = 0

Queue Structure:
front = NULL
rear = NULL
```

---

## Conclusion

This program successfully implements a **Patient Queue System** using a Queue data structure with the following key achievements:

1. **FIFO Principle Implementation**:

   - First-Come, First-Served basis
   - Patients are served in the order they arrive
   - Fair scheduling mechanism

2. **Core Queue Operations**:

   - **Enqueue**: O(1) - Add patient at rear
   - **Dequeue**: O(1) - Remove patient from front
   - **Peek**: O(1) - View front patient without removal
   - **Display**: O(n) - Show all waiting patients
   - **Count**: O(1) - Track using counter variable

3. **Patient Data Management**:

   - Patient ID for unique identification
   - Name, age, and ailment tracking
   - Priority levels (Normal/Urgent)
   - Position tracking in queue

4. **Queue Properties**:

   - **Front pointer**: Points to first patient (next to be served)
   - **Rear pointer**: Points to last patient (most recently added)
   - **Linear structure**: Maintains order of arrival

5. **Real-world Application Features**:

   - Check-in system for new patients
   - Doctor assignment mechanism
   - Waiting list visualization
   - Queue status monitoring

6. **Error Handling**:
   - Empty queue detection
   - Underflow prevention
   - Null pointer checks

**Time Complexity**:

- Enqueue: O(1)
- Dequeue: O(1)
- Peek: O(1)
- Display: O(n)
- Count: O(1)

**Space Complexity**: O(n) where n is number of patients

**Applications**:

- Hospital patient management
- Clinic appointment systems
- Emergency room triage
- Customer service queues
- Resource scheduling systems

**Advantages over Array Implementation**:

- No size limitation
- Dynamic memory allocation
- Efficient insertion and deletion
- No wasted space
