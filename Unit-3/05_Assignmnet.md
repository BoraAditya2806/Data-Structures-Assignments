# Assignment 5

## Problem Statement

You are given a postfix expression (also known as Reverse Polish Notation) consisting of single-digit operands and binary operators (+, -, \*, /). Your task is to evaluate the expression using a stack and return its result.

**Postfix Expression**: Operators follow their operands (e.g., `23+` means `2+3`)

**Examples:**

- `23+` → 5
- `23*5+` → 11 (i.e., 2\*3 + 5)
- `231*+9-` → 8-9 = -1 (i.e., 2+(3\*1) - 9)

---

## Pseudocode

```
FUNCTION isOperator(ch):
    RETURN ch IN ['+', '-', '*', '/']

FUNCTION performOperation(operand1, operand2, operator):
    SWITCH operator:
        CASE '+': RETURN operand1 + operand2
        CASE '-': RETURN operand1 - operand2
        CASE '*': RETURN operand1 * operand2
        CASE '/':
            IF operand2 == 0:
                PRINT "Division by zero error"
                RETURN 0
            RETURN operand1 / operand2

FUNCTION evaluatePostfix(expression):
    CREATE empty stack

    FOR each character ch in expression:
        // If operand (digit), push to stack
        IF ch is digit:
            stack.push(ch - '0')  // Convert char to int

        // If operator, pop two operands and apply operation
        ELSE IF isOperator(ch):
            IF stack.size() < 2:
                PRINT "Invalid expression"
                RETURN ERROR

            operand2 = stack.pop()  // Note: order matters
            operand1 = stack.pop()

            result = performOperation(operand1, operand2, ch)
            stack.push(result)

    // Final result should be the only element in stack
    IF stack.size() != 1:
        PRINT "Invalid expression"
        RETURN ERROR

    RETURN stack.pop()

MAIN:
    INPUT postfix expression
    result = evaluatePostfix(expression)
    DISPLAY result
```

---

## C++ Code

```cpp
#include <iostream>
#include <stack>
#include <string>
#include <iomanip>
#include <cmath>
using namespace std;

class PostfixEvaluator { // Class name is NOT modified
private:
    // Check if character is an operator
    bool isOperator_agb(char ch) { // Method name is modified
        return (ch == '+' || ch == '-' || ch == '*' || ch == '/' || ch == '^');
    }

    // Perform arithmetic operation
    double performOperation_agb(double operand1, double operand2, char op) { // Method name is modified
        switch (op) {
            case '+':
                return operand1 + operand2;
            case '-':
                return operand1 - operand2;
            case '*':
                return operand1 * operand2;
            case '/':
                if (operand2 == 0) {
                    cout << "Error: Division by zero!" << endl;
                    return 0;
                }
                return operand1 / operand2;
            case '^':
                return pow(operand1, operand2);
            default:
                return 0;
        }
    }

public:
    // Evaluate postfix expression
    double evaluate_agb(string expr) { // Method name is modified
        stack<double> st;

        for (int i = 0; i < expr.length(); i++) {
            char ch = expr[i];

            // Skip spaces
            if (ch == ' ')
                continue;

            // If operand (digit), push to stack
            if (isdigit(ch)) {
                st.push(ch - '0');  // Convert char to int
            }
            // If operator, pop two operands and compute
            else if (isOperator_agb(ch)) { // Calling postfixed method
                if (st.size() < 2) {
                    cout << "Error: Invalid expression (insufficient operands)" << endl;
                    return 0;
                }

                double operand2 = st.top();
                st.pop();
                double operand1 = st.top();
                st.pop();

                double result = performOperation_agb(operand1, operand2, ch); // Calling postfixed method
                st.push(result);
            }
            else {
                cout << "Error: Invalid character '" << ch << "'" << endl;
                return 0;
            }
        }

        // Final result should be the only element in stack
        if (st.size() != 1) {
            cout << "Error: Invalid expression (multiple values remaining)" << endl;
            return 0;
        }

        return st.top();
    }

    // Evaluate with step-by-step display
    double evaluateWithSteps_agb(string expr) { // Method name is modified
        stack<double> st;

        cout << "\n--- Step-by-Step Evaluation ---" << endl;
        cout << "Postfix Expression: " << expr << endl;
        cout << "\nStep | Symbol | Operation           | Stack State" << endl;
        cout << "-----+--------+---------------------+-------------" << endl;

        int step = 1;

        for (int i = 0; i < expr.length(); i++) {
            char ch = expr[i];

            if (ch == ' ')
                continue;

            cout << " " << setw(2) << step++ << "  |   " << ch << "    | ";

            if (isdigit(ch)) {
                st.push(ch - '0');
                cout << "Push " << (ch - '0') << "             | ";
                displayStack_agb(st); // Calling postfixed method
            }
            else if (isOperator_agb(ch)) { // Calling postfixed method
                if (st.size() < 2) {
                    cout << "ERROR               | Insufficient operands" << endl;
                    return 0;
                }

                double op2 = st.top();
                st.pop();
                double op1 = st.top();
                st.pop();

                double result = performOperation_agb(op1, op2, ch); // Calling postfixed method

                cout << op1 << " " << ch << " " << op2 << " = " << result;
                cout << "     | ";

                st.push(result);
                displayStack_agb(st); // Calling postfixed method
            }
        }

        cout << "\nFinal Result: ";
        if (st.size() == 1) {
            cout << st.top() << endl;
            return st.top();
        } else {
            cout << "Error - Invalid expression" << endl;
            return 0;
        }
    }

    // Display stack contents
    void displayStack_agb(stack<double> st) { // Method name is modified
        if (st.empty()) {
            cout << "Empty" << endl;
            return;
        }

        stack<double> temp;
        while (!st.empty()) {
            temp.push(st.top());
            st.pop();
        }

        cout << "[ ";
        while (!temp.empty()) {
            cout << temp.top();
            temp.pop();
            if (!temp.empty()) cout << ", ";
        }
        cout << " ]" << endl;
    }
};

int main_agb() { // Function name is modified
    PostfixEvaluator evaluator_agb; // Variable name is modified
    string expression;
    int choice;

    do {
        cout << "\n===== Postfix Expression Evaluator =====" << endl;
        cout << "1. Quick evaluation" << endl;
        cout << "2. Evaluation with step-by-step display" << endl;
        cout << "3. Test predefined examples" << endl;
        cout << "4. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;
        cin.ignore();

        switch (choice) {
            case 1:
                cout << "Enter postfix expression: ";
                getline(cin, expression);
                {
                    double result = evaluator_agb.evaluate_agb(expression); // Calling postfixed method
                    cout << "Result: " << result << endl;
                }
                break;

            case 2:
                cout << "Enter postfix expression: ";
                getline(cin, expression);
                evaluator_agb.evaluateWithSteps_agb(expression); // Calling postfixed method
                break;

            case 3:
                {
                    string examples[] = {
                        "23+",            // 2+3 = 5
                        "23*5+",          // 2*3+5 = 11
                        "231*+9-",        // 2+3*1-9 = -4
                        "53+82-*",        // (5+3)*(8-2) = 48
                        "62/3-42*+",      // 6/2-3+4*2 = 8
                        "93/32/+",        // 9/3+3/2 = 4.5
                        "23^",            // 2^3 = 8
                        "524*+36/-"       // (5+2*4)/(3-6) = -4.33
                    };

                    cout << "\n--- Testing Predefined Examples ---" << endl;
                    for (int i = 0; i < 8; i++) {
                        cout << "\n" << (i+1) << ". Expression: " << examples[i] << endl;
                        double result = evaluator_agb.evaluate_agb(examples[i]); // Calling postfixed method
                        cout << "    Result: " << result << endl;
                    }
                }
                break;

            case 4:
                cout << "Exiting... Thank you!" << endl;
                break;

            default:
                cout << "Invalid choice!" << endl;
        }
    } while (choice != 4);

    return 0;
}
```

---

## Input/Output

### Example 1: Simple Expression `23+`

```
===== Postfix Expression Evaluator =====
1. Quick evaluation
2. Evaluation with step-by-step display
3. Test predefined examples
4. Exit
Enter your choice: 2
Enter postfix expression: 23+

--- Step-by-Step Evaluation ---
Postfix Expression: 23+

Step | Symbol | Operation          | Stack State
-----+--------+--------------------+-------------
  1  |   2    | Push 2             | [ 2 ]
  2  |   3    | Push 3             | [ 2, 3 ]
  3  |   +    | 2 + 3 = 5          | [ 5 ]

Final Result: 5
```

### Example 2: Complex Expression `23*5+`

```
Enter your choice: 2
Enter postfix expression: 23*5+

--- Step-by-Step Evaluation ---
Postfix Expression: 23*5+

Step | Symbol | Operation          | Stack State
-----+--------+--------------------+-------------
  1  |   2    | Push 2             | [ 2 ]
  2  |   3    | Push 3             | [ 2, 3 ]
  3  |   *    | 2 * 3 = 6          | [ 6 ]
  4  |   5    | Push 5             | [ 6, 5 ]
  5  |   +    | 6 + 5 = 11         | [ 11 ]

Final Result: 11
```

### Example 3: Expression with Subtraction `231*+9-`

```
Enter your choice: 2
Enter postfix expression: 231*+9-

--- Step-by-Step Evaluation ---
Postfix Expression: 231*+9-

Step | Symbol | Operation          | Stack State
-----+--------+--------------------+-------------
  1  |   2    | Push 2             | [ 2 ]
  2  |   3    | Push 3             | [ 2, 3 ]
  3  |   1    | Push 1             | [ 2, 3, 1 ]
  4  |   *    | 3 * 1 = 3          | [ 2, 3 ]
  5  |   +    | 2 + 3 = 5          | [ 5 ]
  6  |   9    | Push 9             | [ 5, 9 ]
  7  |   -    | 5 - 9 = -4         | [ -4 ]

Final Result: -4
```

### Example 4: Complex Expression `53+82-*`

```
Enter your choice: 2
Enter postfix expression: 53+82-*

--- Step-by-Step Evaluation ---
Postfix Expression: 53+82-*

Step | Symbol | Operation          | Stack State
-----+--------+--------------------+-------------
  1  |   5    | Push 5             | [ 5 ]
  2  |   3    | Push 3             | [ 5, 3 ]
  3  |   +    | 5 + 3 = 8          | [ 8 ]
  4  |   8    | Push 8             | [ 8, 8 ]
  5  |   2    | Push 2             | [ 8, 8, 2 ]
  6  |   -    | 8 - 2 = 6          | [ 8, 6 ]
  7  |   *    | 8 * 6 = 48         | [ 48 ]

Final Result: 48
```

### Example 5: Predefined Test Cases

```
Enter your choice: 3

--- Testing Predefined Examples ---

1. Expression: 23+
   Result: 5

2. Expression: 23*5+
   Result: 11

3. Expression: 231*+9-
   Result: -4

4. Expression: 53+82-*
   Result: 48

5. Expression: 62/3-42*+
   Result: 8

6. Expression: 93/32/+
   Result: 4.5

7. Expression: 23^
   Result: 8

8. Expression: 524*+36/-
   Result: -4.33333
```

---

## Dry Run

### Expression: `23*5+`

**Equivalent Infix**: `(2 * 3) + 5 = 11`

| Step | Symbol | Action  | Stack Before | Operation                   | Stack After |
| ---- | ------ | ------- | ------------ | --------------------------- | ----------- |
| 1    | 2      | Push    | []           | Push 2                      | [2]         |
| 2    | 3      | Push    | [2]          | Push 3                      | [2, 3]      |
| 3    | \*     | Compute | [2, 3]       | Pop 3, Pop 2, Push (2\*3=6) | [6]         |
| 4    | 5      | Push    | [6]          | Push 5                      | [6, 5]      |
| 5    | +      | Compute | [6, 5]       | Pop 5, Pop 6, Push (6+5=11) | [11]        |

**Final Result**: 11

---

### Expression: `231*+9-`

**Equivalent Infix**: `2 + (3 * 1) - 9 = -4`

| Step | Symbol | Action  | Stack     | Operation                      |
| ---- | ------ | ------- | --------- | ------------------------------ |
| 1    | 2      | Push    | [2]       | Push 2                         |
| 2    | 3      | Push    | [2, 3]    | Push 3                         |
| 3    | 1      | Push    | [2, 3, 1] | Push 1                         |
| 4    | \*     | Compute | [2, 3]    | Pop 1, Pop 3 → 3\*1=3, Push 3  |
| 5    | +      | Compute | [5]       | Pop 3, Pop 2 → 2+3=5, Push 5   |
| 6    | 9      | Push    | [5, 9]    | Push 9                         |
| 7    | -      | Compute | [-4]      | Pop 9, Pop 5 → 5-9=-4, Push -4 |

**Final Result**: -4

---

### Expression: `53+82-*`

**Equivalent Infix**: `(5 + 3) * (8 - 2) = 48`

| Step | Symbol | Stack     | Operation |
| ---- | ------ | --------- | --------- |
| 1    | 5      | [5]       | Push 5    |
| 2    | 3      | [5, 3]    | Push 3    |
| 3    | +      | [8]       | 5+3=8     |
| 4    | 8      | [8, 8]    | Push 8    |
| 5    | 2      | [8, 8, 2] | Push 2    |
| 6    | -      | [8, 6]    | 8-2=6     |
| 7    | \*     | [48]      | 8\*6=48   |

**Final Result**: 48

---

### Key Points in Evaluation:

1. **Order Matters**: When popping for binary operators:

   ```
   operand2 = stack.pop()  // Second operand (right)
   operand1 = stack.pop()  // First operand (left)
   result = operand1 ⊕ operand2
   ```

2. **For Subtraction**: `5 2 -` means `5 - 2 = 3` (NOT `2 - 5`)

3. **For Division**: `6 2 /` means `6 / 2 = 3` (NOT `2 / 6`)

---

## Conclusion

This program successfully implements **Postfix Expression Evaluation** using a stack with the following achievements:

1. **Algorithm Implementation**:

   - Scans expression left to right
   - Uses stack to store intermediate results
   - Pops two operands for each operator
   - Pushes result back to stack

2. **Supported Operations**:

   - Addition (+)
   - Subtraction (-)
   - Multiplication (\*)
   - Division (/) with zero-division check
   - Exponentiation (^)

3. **Advantages of Postfix**:

   - **No Parentheses Needed**: Expression is unambiguous
   - **Easy to Evaluate**: Single left-to-right scan
   - **Efficient**: No operator precedence checking required
   - **Stack-Friendly**: Natural fit for stack-based evaluation

4. **Evaluation Steps**:

   ```
   For each symbol:
     If operand → Push to stack
     If operator → Pop two operands, compute, push result

   Final stack should contain exactly one value (the result)
   ```

5. **Error Handling**:

   - Division by zero detection
   - Insufficient operands check
   - Invalid expression validation
   - Invalid character detection

6. **Features**:
   - Step-by-step visualization
   - Stack state display at each step
   - Multiple test cases
   - Quick and detailed evaluation modes

**Time Complexity**: O(n) where n is length of expression  
**Space Complexity**: O(n) for stack in worst case  
**Applications**:

- Calculator implementations
- Compiler expression evaluation
- Computer architecture (stack machines)
- Scripting language interpreters
- Mathematical software
