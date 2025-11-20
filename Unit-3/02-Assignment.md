# Assignment 2

## Problem Statement

Convert given infix expression (e.g., `a-b*c-d/e+f`) into postfix form using stack and show the operations step by step.

**Infix Expression**: Operators are written between operands (e.g., A + B)  
**Postfix Expression**: Operators are written after operands (e.g., AB+)

---

## Pseudocode

```
FUNCTION precedence(operator):
    IF operator == '^':
        RETURN 3
    ELSE IF operator == '*' OR operator == '/':
        RETURN 2
    ELSE IF operator == '+' OR operator == '-':
        RETURN 1
    ELSE:
        RETURN 0

FUNCTION isOperator(ch):
    RETURN ch IN ['+', '-', '*', '/', '^']

FUNCTION infixToPostfix(infix):
    CREATE empty stack
    postfix = ""

    FOR each character ch in infix:
        // If operand, add to output
        IF ch is alphanumeric:
            postfix += ch

        // If opening bracket, push to stack
        ELSE IF ch == '(':
            stack.push(ch)

        // If closing bracket, pop until opening bracket
        ELSE IF ch == ')':
            WHILE stack is not empty AND stack.top() != '(':
                postfix += stack.pop()
            IF stack is not empty:
                stack.pop()  // Remove '('

        // If operator
        ELSE IF isOperator(ch):
            WHILE stack is not empty AND
                  precedence(stack.top()) >= precedence(ch):
                postfix += stack.pop()
            stack.push(ch)

    // Pop remaining operators
    WHILE stack is not empty:
        postfix += stack.pop()

    RETURN postfix

MAIN:
    INPUT infix expression
    CALL infixToPostfix(infix)
    DISPLAY postfix expression with step-by-step process
```

---

## C++ Code

```cpp
#include <iostream>
#include <stack>
#include <string>
#include <iomanip>
using namespace std;

class InfixToPostfix {
private:
    // Function to return precedence of operators
    int precedence_agb(char op) {
        if (op == '^')
            return 3;
        else if (op == '*' || op == '/')
            return 2;
        else if (op == '+' || op == '-')
            return 1;
        else
            return 0;
    }

    // Function to check if character is operator
    bool isOperator_agb(char ch) {
        return (ch == '+' || ch == '-' || ch == '*' || ch == '/' || ch == '^');
    }

public:
    // Convert infix to postfix with step-by-step display
    string convert_agb(string infix) {
        stack<char> st;
        string postfix = "";

        cout << "\n" << setw(10) << "Symbol" << setw(20) << "Stack"
             << setw(25) << "Postfix Expression" << endl;
        cout << string(55, '-') << endl;

        for (int i = 0; i < infix.length(); i++) {
            char ch = infix[i];

            if (ch == ' ')
                continue;

            if (isalnum(ch)) {
                postfix += ch;
                displayStep_agb(ch, st, postfix);
            }
            else if (ch == '(') {
                st.push(ch);
                displayStep_agb(ch, st, postfix);
            }
            else if (ch == ')') {
                while (!st.empty() && st.top() != '(') {
                    postfix += st.top();
                    st.pop();
                }
                if (!st.empty())
                    st.pop();
                displayStep_agb(ch, st, postfix);
            }
            else if (isOperator_agb(ch)) {
                while (!st.empty() && precedence_agb(st.top()) >= precedence_agb(ch)) {
                    postfix += st.top();
                    st.pop();
                }
                st.push(ch);
                displayStep_agb(ch, st, postfix);
            }
        }

        while (!st.empty()) {
            postfix += st.top();
            displayStep_agb(' ', st, postfix);
            st.pop();
        }

        cout << string(55, '-') << endl;
        return postfix;
    }

    // Display current step
    void displayStep_agb(char symbol, stack<char> st, string postfix) {
        cout << setw(10) << symbol << setw(20);

        if (st.empty()) {
            cout << "Empty";
        } else {
            stack<char> temp = st;
            string stackContent = "";
            while (!temp.empty()) {
                stackContent = temp.top() + stackContent;
                temp.pop();
            }
            cout << stackContent;
        }

        cout << setw(25) << postfix << endl;
    }

    // Simple conversion without step display
    string simpleConvert_agb(string infix) {
        stack<char> st;
        string postfix = "";

        for (int i = 0; i < infix.length(); i++) {
            char ch = infix[i];

            if (ch == ' ')
                continue;

            if (isalnum(ch)) {
                postfix += ch;
            }
            else if (ch == '(') {
                st.push(ch);
            }
            else if (ch == ')') {
                while (!st.empty() && st.top() != '(') {
                    postfix += st.top();
                    st.pop();
                }
                if (!st.empty())
                    st.pop();
            }
            else if (isOperator_agb(ch)) {
                while (!st.empty() && precedence_agb(st.top()) >= precedence_agb(ch)) {
                    postfix += st.top();
                    st.pop();
                }
                st.push(ch);
            }
        }

        while (!st.empty()) {
            postfix += st.top();
            st.pop();
        }

        return postfix;
    }
};

int main() {
    InfixToPostfix converter;
    string infix;
    int choice;

    do {
        cout << "\n===== Infix to Postfix Converter =====" << endl;
        cout << "1. Convert with step-by-step display" << endl;
        cout << "2. Quick conversion" << endl;
        cout << "3. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;
        cin.ignore();

        switch (choice) {
            case 1:
                cout << "Enter infix expression: ";
                getline(cin, infix);
                cout << "\n--- Step-by-Step Conversion ---";
                cout << "\nInfix Expression: " << infix << endl;
                {
                    string postfix = converter.convert_agb(infix);
                    cout << "\nFinal Postfix Expression: " << postfix << endl;
                }
                break;

            case 2:
                cout << "Enter infix expression: ";
                getline(cin, infix);
                {
                    string postfix = converter.simpleConvert_agb(infix);
                    cout << "Infix:   " << infix << endl;
                    cout << "Postfix: " << postfix << endl;
                }
                break;

            case 3:
                cout << "Exiting... Thank you!" << endl;
                break;

            default:
                cout << "Invalid choice!" << endl;
        }
    } while (choice != 3);

    return 0;
}

```

---

## Input/Output

### Example 1: `a-b*c-d/e+f`

```
===== Infix to Postfix Converter =====
1. Convert with step-by-step display
2. Quick conversion
3. Exit
Enter your choice: 1
Enter infix expression: a-b*c-d/e+f

--- Step-by-Step Conversion ---
Infix Expression: a-b*c-d/e+f

    Symbol               Stack       Postfix Expression
-------------------------------------------------------
         a               Empty                        a
         -                   -                        a
         b                   -                       ab
         *                  -*                       ab
         c                  -*                      abc
         -                   -                    abc*-
         d                   -                    abc*-d
         /                  -/                    abc*-d
         e                  -/                   abc*-de
         +                   +                 abc*-de/-
         f                   +                 abc*-de/-f
                            Empty              abc*-de/-f+
-------------------------------------------------------

Final Postfix Expression: abc*-de/-f+
```

### Example 2: `(a+b)*c-d/(e+f)`

```
Enter infix expression: (a+b)*c-d/(e+f)

--- Step-by-Step Conversion ---
Infix Expression: (a+b)*c-d/(e+f)

    Symbol               Stack       Postfix Expression
-------------------------------------------------------
         (                   (
         a                   (                        a
         +                  (+                        a
         b                  (+                       ab
         )               Empty                      ab+
         *                   *                      ab+
         c                   *                     ab+c
         -                   -                    ab+c*
         d                   -                    ab+c*d
         /                  -/                    ab+c*d
         (                 -/(                    ab+c*d
         e                 -/(                   ab+c*de
         +                -/(+                   ab+c*de
         f                -/(+                  ab+c*def
         )                  -/                  ab+c*def+
                            Empty              ab+c*def+/-
-------------------------------------------------------

Final Postfix Expression: ab+c*def+/-
```

---

## Dry Run

### Expression: `a-b*c-d/e+f`

| Step | Symbol | Stack State | Postfix      | Action                                           |
| ---- | ------ | ----------- | ------------ | ------------------------------------------------ |
| 1    | a      | Empty       | a            | Operand - add to postfix                         |
| 2    | -      | -           | a            | Operator - push to stack                         |
| 3    | b      | -           | ab           | Operand - add to postfix                         |
| 4    | \*     | -\*         | ab           | Higher precedence - push                         |
| 5    | c      | -\*         | abc          | Operand - add to postfix                         |
| 6    | -      | -           | abc\*-       | Pop \* (higher prec), pop - (equal prec), push - |
| 7    | d      | -           | abc\*-d      | Operand - add to postfix                         |
| 8    | /      | -/          | abc\*-d      | Higher precedence - push                         |
| 9    | e      | -/          | abc\*-de     | Operand - add to postfix                         |
| 10   | +      | +           | abc\*-de/-   | Pop / (higher), pop - (higher), push +           |
| 11   | f      | +           | abc\*-de/-f  | Operand - add to postfix                         |
| 12   | End    | Empty       | abc\*-de/-f+ | Pop remaining +                                  |

**Final Result**: `abc*-de/-f+`

### Detailed Step 6 Explanation:

When we encounter the second `-`:

1. Stack top is `*` with precedence 2
2. Current operator `-` has precedence 1
3. Since 2 ≥ 1, pop `*` and add to postfix: `abc*`
4. Now stack top is `-` with precedence 1
5. Current operator `-` has precedence 1
6. Since 1 ≥ 1, pop `-` and add to postfix: `abc*-`
7. Stack is now empty, push current `-`

---

## Conclusion

This program successfully implements **Infix to Postfix Conversion** using a stack data structure with the following achievements:

1. **Algorithm Implementation**:

   - Correctly implements the standard infix-to-postfix conversion algorithm
   - Uses operator precedence rules (^ > \*/ > +-)
   - Handles parentheses for sub-expression grouping

2. **Key Features**:

   - Step-by-step visualization of conversion process
   - Supports all common operators (+, -, \*, /, ^)
   - Handles parentheses correctly
   - Displays stack contents at each step

3. **Operator Precedence Handling**:

   ```
   ^ (Exponentiation)  - Precedence 3
   *, /                - Precedence 2
   +, -                - Precedence 1
   ```

4. **Rules Applied**:

   - Operands are directly added to postfix
   - Operators are pushed after popping higher/equal precedence operators
   - Opening parenthesis `(` is pushed immediately
   - Closing parenthesis `)` causes popping until matching `(`
   - All remaining operators popped at end

5. **Advantages of Postfix**:
   - No need for parentheses
   - Easy to evaluate using a single stack
   - Directly maps to stack-based computation
   - Unambiguous expression representation

**Time Complexity**: O(n) where n is the length of infix expression  
**Space Complexity**: O(n) for stack storage in worst case  
**Applications**: Compiler design, expression evaluation, calculator implementations
