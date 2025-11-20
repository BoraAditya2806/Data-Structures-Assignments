# Assignment 7

## Problem Statement

Pizza parlour accepting maximum n orders. Orders are served on an FCFS (First-Come, First-Served) basis. Order once placed can't be cancelled. Write a C++ program to simulate the system using circular QUEUE.

---

## Pseudocode

```
CLASS Order:
    orderID: integer
    customerName: string
    pizzaType: string
    quantity: integer

CLASS CircularQueue:
    queue[MAX_SIZE]
    front: integer
    rear: integer
    count: integer
    MAX_SIZE: constant

    FUNCTION __init__(size):
        MAX_SIZE = size
        front = -1
        rear = -1
        count = 0
        CREATE array of size MAX_SIZE

    FUNCTION isFull():
        RETURN count == MAX_SIZE

    FUNCTION isEmpty():
        RETURN count == 0

    FUNCTION enqueue(order):
        IF isFull():
            PRINT "Order queue full! Cannot accept more orders"
            RETURN FALSE

        IF front == -1:
            front = 0

        rear = (rear + 1) % MAX_SIZE
        queue[rear] = order
        count++
        PRINT "Order placed successfully"
        RETURN TRUE

    FUNCTION dequeue():
        IF isEmpty():
            PRINT "No orders to serve"
            RETURN NULL

        order = queue[front]

        IF front == rear:  // Only one element
            front = rear = -1
        ELSE:
            front = (front + 1) % MAX_SIZE

        count--
        RETURN order

    FUNCTION display():
        IF isEmpty():
            PRINT "No pending orders"
            RETURN

        i = front
        FOR count times:
            PRINT queue[i]
            i = (i + 1) % MAX_SIZE

MAIN:
    GET maximum capacity
    CREATE CircularQueue with capacity
    LOOP:
        DISPLAY menu
        GET choice
        SWITCH choice:
            CASE 1: Place order (enqueue)
            CASE 2: Serve order (dequeue)
            CASE 3: Display pending orders
            CASE 4: Exit
```

---

## C++ Code

```cpp
#include <iostream>
#include <string>
#include <iomanip>
using namespace std;

class Order {
public:
    int orderID;
    string customerName;
    string pizzaType;
    int quantity;

    Order_agb() {
        orderID = 0;
        customerName = "";
        pizzaType = "";
        quantity = 0;
    }

    Order_agb(int id, string name, string type, int qty) {
        orderID = id;
        customerName = name;
        pizzaType = type;
        quantity = qty;
    }

    void display_agb() {
        cout << setw(8) << orderID
             << setw(20) << customerName
             << setw(20) << pizzaType
             << setw(10) << quantity << endl;
    }
};

class CircularQueue {
private:
    Order* queue;
    int front;
    int rear;
    int MAX_SIZE;
    int count;

public:
    CircularQueue_agb(int size) {
        MAX_SIZE = size;
        queue = new Order[MAX_SIZE];
        front = -1;
        rear = -1;
        count = 0;
        cout << "Pizza Parlour initialized with capacity: " << MAX_SIZE << " orders" << endl;
    }

    bool isFull_agb() {
        return count == MAX_SIZE;
    }

    bool isEmpty_agb() {
        return count == 0;
    }

    bool placeOrder_agb(Order order) {
        if (isFull_agb()) {
            cout << "\n✗ Sorry! Order queue is FULL." << endl;
            cout << "Cannot accept more orders at the moment." << endl;
            cout << "Please wait for current orders to be served." << endl;
            return false;
        }

        if (front == -1) {
            front = 0;
        }

        rear = (rear + 1) % MAX_SIZE;
        queue[rear] = order;
        count++;

        cout << "\n✓ Order #" << order.orderID << " placed successfully!" << endl;
        cout << "Customer: " << order.customerName << endl;
        cout << "Pizza: " << order.pizzaType << " x " << order.quantity << endl;
        cout << "Position in queue: " << count << endl;
        return true;
    }

    Order serveOrder_agb() {
        if (isEmpty_agb()) {
            cout << "\n✗ No pending orders to serve!" << endl;
            return Order_agb();
        }

        Order order = queue[front];

        if (front == rear) {
            front = rear = -1;
        } else {
            front = (front + 1) % MAX_SIZE;
        }

        count--;

        cout << "\n✓ Serving Order #" << order.orderID << endl;
        cout << "Customer: " << order.customerName << endl;
        cout << "Pizza: " << order.pizzaType << " x " << order.quantity << endl;
        cout << "Remaining orders: " << count << endl;

        return order;
    }

    void displayOrders_agb() {
        if (isEmpty_agb()) {
            cout << "\n✓ No pending orders! Queue is empty." << endl;
            return;
        }

        cout << "\n========== Pending Orders ==========" << endl;
        cout << setw(8) << "OrderID"
             << setw(20) << "Customer"
             << setw(20) << "Pizza Type"
             << setw(10) << "Quantity" << endl;
        cout << string(58, '-') << endl;

        int i = front;
        int displayCount = 0;

        while (displayCount < count) {
            cout << "Order " << (displayCount + 1) << ": ";
            queue[i].display_agb();
            i = (i + 1) % MAX_SIZE;
            displayCount++;
        }

        cout << string(58, '=') << endl;
        cout << "Total pending orders: " << count << "/" << MAX_SIZE << endl;
        cout << "Available slots: " << (MAX_SIZE - count) << endl;
    }

    void displayStatus_agb() {
        cout << "\n--- Queue Status ---" << endl;
        cout << "Capacity: " << MAX_SIZE << endl;
        cout << "Current orders: " << count << endl;
        cout << "Available slots: " << (MAX_SIZE - count) << endl;
        cout << "Front index: " << front << endl;
        cout << "Rear index: " << rear << endl;

        if (isFull_agb()) {
            cout << "Status: FULL ⚠" << endl;
        } else if (isEmpty_agb()) {
            cout << "Status: EMPTY ✓" << endl;
        } else {
            cout << "Status: ACTIVE" << endl;
        }
    }

    ~CircularQueue_agb() {
        delete[] queue;
    }
};

int main_agb() {
    int capacity;

    cout << "===== Pizza Parlour Order Management System =====" << endl;
    cout << "Enter maximum order capacity: ";
    cin >> capacity;

    CircularQueue_agb parlour_agb(capacity);

    int choice;
    int orderCounter_agb = 1;

    do {
        cout << "\n===== Main Menu =====" << endl;
        cout << "1. Place new order" << endl;
        cout << "2. Serve next order" << endl;
        cout << "3. Display pending orders" << endl;
        cout << "4. Display queue status" << endl;
        cout << "5. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;
        cin.ignore();

        switch (choice) {
            case 1: {
                string name, type;
                int qty;

                cout << "\n--- Place New Order ---" << endl;
                cout << "Enter customer name: ";
                getline(cin, name);
                cout << "Enter pizza type (Margherita/Pepperoni/Veggie/BBQ): ";
                getline(cin, type);
                cout << "Enter quantity: ";
                cin >> qty;

                Order order(orderCounter_agb, name, type, qty);
                if (parlour_agb.placeOrder_agb(order)) {
                    orderCounter_agb++;
                }
                break;
            }

            case 2:
                parlour_agb.serveOrder_agb();
                break;

            case 3:
                parlour_agb.displayOrders_agb();
                break;

            case 4:
                parlour_agb.displayStatus_agb();
                break;

            case 5:
                cout << "Closing Pizza Parlour... Thank you!" << endl;
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
===== Pizza Parlour Order Management System =====
Enter maximum order capacity: 5
Pizza Parlour initialized with capacity: 5 orders

===== Main Menu =====
1. Place new order
2. Serve next order
3. Display pending orders
4. Display queue status
5. Exit
Enter your choice: 1

--- Place New Order ---
Enter customer name: Alice Brown
Enter pizza type (Margherita/Pepperoni/Veggie/BBQ): Margherita
Enter quantity: 2

✓ Order #1 placed successfully!
Customer: Alice Brown
Pizza: Margherita x 2
Position in queue: 1

Enter your choice: 1

--- Place New Order ---
Enter customer name: Bob Wilson
Enter pizza type (Margherita/Pepperoni/Veggie/BBQ): Pepperoni
Enter quantity: 1

✓ Order #2 placed successfully!
Customer: Bob Wilson
Pizza: Pepperoni x 1
Position in queue: 2

Enter your choice: 1

--- Place New Order ---
Enter customer name: Charlie Davis
Enter pizza type (Margherita/Pepperoni/Veggie/BBQ): BBQ
Enter quantity: 3

✓ Order #3 placed successfully!
Customer: Charlie Davis
Pizza: BBQ x 3
Position in queue: 3

Enter your choice: 3

========== Pending Orders ==========
 OrderID            Customer          Pizza Type  Quantity
----------------------------------------------------------
Order 1:       1        Alice Brown          Margherita         2
Order 2:       2         Bob Wilson           Pepperoni         1
Order 3:       3      Charlie Davis                 BBQ         3
==========================================================
Total pending orders: 3/5
Available slots: 2

Enter your choice: 2

✓ Serving Order #1
Customer: Alice Brown
Pizza: Margherita x 2
Remaining orders: 2

Enter your choice: 1

--- Place New Order ---
Enter customer name: Diana Prince
Enter pizza type (Margherita/Pepperoni/Veggie/BBQ): Veggie
Enter quantity: 2

✓ Order #4 placed successfully!
Customer: Diana Prince
Pizza: Veggie x 2
Position in queue: 3

Enter your choice: 4

--- Queue Status ---
Capacity: 5
Current orders: 3
Available slots: 2
Front index: 1
Rear index: 3
Status: ACTIVE

Enter your choice: 1

--- Place New Order ---
Enter customer name: Eve Miller
Enter pizza type (Margherita/Pepperoni/Veggie/BBQ): Margherita
Enter quantity: 1

✓ Order #5 placed successfully!
Customer: Eve Miller
Pizza: Margherita x 1
Position in queue: 4

Enter your choice: 1

--- Place New Order ---
Enter customer name: Frank Castle
Enter pizza type (Margherita/Pepperoni/Veggie/BBQ): Pepperoni
Enter quantity: 2

✓ Order #6 placed successfully!
Customer: Frank Castle
Pizza: Pepperoni x 2
Position in queue: 5

Enter your choice: 1

--- Place New Order ---
Enter customer name: Grace Lee
Enter pizza type (Margherita/Pepperoni/Veggie/BBQ): BBQ
Enter quantity: 1

✗ Sorry! Order queue is FULL.
Cannot accept more orders at the moment.
Please wait for current orders to be served.

Enter your choice: 2

✓ Serving Order #2
Customer: Bob Wilson
Pizza: Pepperoni x 1
Remaining orders: 4

Enter your choice: 3

========== Pending Orders ==========
 OrderID            Customer          Pizza Type  Quantity
----------------------------------------------------------
Order 1:       3      Charlie Davis                 BBQ         3
Order 2:       4       Diana Prince              Veggie         2
Order 3:       5         Eve Miller          Margherita         1
Order 4:       6      Frank Castle           Pepperoni         2
==========================================================
Total pending orders: 4/5
Available slots: 1

Enter your choice: 5
Closing Pizza Parlour... Thank you!
```

---

## Dry Run

### Configuration: MAX_SIZE = 5

### Initial State:

```
front = -1
rear = -1
count = 0
Array indices: [0] [1] [2] [3] [4]
```

### Operation 1: placeOrder(Order #1 - Alice)

```
Step 1: isFull()? count(0) == 5? NO
Step 2: front == -1? YES → front = 0
Step 3: rear = (−1 + 1) % 5 = 0
Step 4: queue[0] = Order #1
Step 5: count = 1

State:
front = 0, rear = 0, count = 1
[Alice] [ ] [ ] [ ] [ ]
  ^F,R
```

### Operation 2: placeOrder(Order #2 - Bob)

```
Step 1: isFull()? NO
Step 2: front == -1? NO
Step 3: rear = (0 + 1) % 5 = 1
Step 4: queue[1] = Order #2
Step 5: count = 2

State:
front = 0, rear = 1, count = 2
[Alice] [Bob] [ ] [ ] [ ]
  ^F      ^R
```

### Operation 3: placeOrder(Order #3 - Charlie)

```
rear = (1 + 1) % 5 = 2
queue[2] = Order #3
count = 3

State:
front = 0, rear = 2, count = 3
[Alice] [Bob] [Charlie] [ ] [ ]
  ^F             ^R
```

### Operation 4: serveOrder()

```
Step 1: isEmpty()? NO
Step 2: order = queue[0] (Alice)
Step 3: front == rear? 0 == 2? NO
Step 4: front = (0 + 1) % 5 = 1
Step 5: count = 2

State:
front = 1, rear = 2, count = 2
[ ] [Bob] [Charlie] [ ] [ ]
     ^F      ^R
```

### Operation 5: placeOrder(Order #4 - Diana)

```
rear = (2 + 1) % 5 = 3
queue[3] = Order #4
count = 3

State:
front = 1, rear = 3, count = 3
[ ] [Bob] [Charlie] [Diana] [ ]
     ^F                ^R
```

### Operations 6-7: placeOrder(Order #5 - Eve), placeOrder(Order #6 - Frank)

```
After Order #5:
rear = (3 + 1) % 5 = 4
count = 4

After Order #6:
rear = (4 + 1) % 5 = 0  ← WRAPS AROUND
count = 5

State:
front = 1, rear = 0, count = 5
[Frank] [Bob] [Charlie] [Diana] [Eve]
   ^R     ^F
```

### Operation 8: placeOrder(Order #7 - Grace) - OVERFLOW

```
Step 1: isFull()? count(5) == 5? YES
Display: "Order queue is FULL"
Return FALSE

No changes to queue
```

### Operation 9: serveOrder()

```
order = queue[1] (Bob)
front = (1 + 1) % 5 = 2
count = 4

State:
front = 2, rear = 0, count = 4
[Frank] [ ] [Charlie] [Diana] [Eve]
   ^R          ^F
```

---

## Conclusion

This program successfully implements a **Pizza Parlour Order Management System** using a **Circular Queue** with the following key achievements:

1. **Circular Queue Implementation**:

   - Efficient use of array space
   - Wrap-around mechanism using modulo operator
   - No wasted space after dequeue operations
   - Fixed capacity with overflow handling

2. **Circular Queue Formula**:

   ```
   Enqueue: rear = (rear + 1) % MAX_SIZE
   Dequeue: front = (front + 1) % MAX_SIZE
   ```

3. **Core Operations**:

   - **placeOrder (Enqueue)**: O(1) - Add order at rear
   - **serveOrder (Dequeue)**: O(1) - Remove order from front
   - **isFull**: O(1) - Check capacity
   - **isEmpty**: O(1) - Check empty status
   - **Display**: O(n) - Show all orders

4. **Business Rules Implemented**:

   - FCFS (First-Come, First-Served) principle
   - Orders cannot be cancelled once placed
   - Maximum capacity enforcement
   - Order tracking with unique IDs

5. **Circular Queue Advantages**:

   - **Space Efficiency**: Reuses freed positions
   - **No Shifting**: Unlike linear queue, no element shifting needed
   - **Constant Time**: All operations are O(1)
   - **Fixed Memory**: Predictable memory usage

6. **Wrap-Around Mechanism**:

   ```
   Indices: 0 → 1 → 2 → 3 → 4 → 0 → 1 → ...
            └────────────────────┘
                 Circular
   ```

7. **Queue States**:
   - **Empty**: `count == 0` or `front == -1`
   - **Full**: `count == MAX_SIZE`
   - **Active**: `0 < count < MAX_SIZE`

**Time Complexity**:

- Enqueue: O(1)
- Dequeue: O(1)
- isFull/isEmpty: O(1)
- Display: O(n)

**Space Complexity**: O(n) where n is MAX_SIZE

**Applications**:

- Restaurant order management
- Print job scheduling
- Round-robin CPU scheduling
- Buffering systems
- Token management systems

**Circular Queue vs Linear Queue**:
| Feature | Linear Queue | Circular Queue |
|---------|-------------|----------------|
| Space utilization | Poor (wasted front space) | Excellent (reuses space) |
| Overflow condition | rear == MAX_SIZE-1 | count == MAX_SIZE |
| Implementation | Simple | Moderate (modulo arithmetic) |
| Real-world suitability | Limited | High |
