# Assignment 4

## Problem Statement

You are given a string containing only parentheses characters: '(', ')', '{', '}', '[', and ']'. Your task is to check whether the parentheses are balanced or not.

A string is considered balanced if:

1. Every opening bracket has a corresponding closing bracket of the same type
2. Brackets are closed in the correct order

**Examples:**

- `"{[()]}"` → Balanced
- `"{[(])}"` → Not Balanced (wrong order)
- `"(()"` → Not Balanced (missing closing bracket)

---

## Pseudocode

```
FUNCTION isMatchingPair(opening, closing):
    IF opening == '(' AND closing == ')':
        RETURN TRUE
    IF opening == '{' AND closing == '}':
        RETURN TRUE
    IF opening == '[' AND closing == ']':
        RETURN TRUE
    RETURN FALSE

FUNCTION isBalanced(expression):
    CREATE empty stack

    FOR each character ch in expression:
        // If opening bracket, push to stack
        IF ch IN ['(', '{', '[']:
            stack.push(ch)

        // If closing bracket
        ELSE IF ch IN [')', '}', ']']:
            // Check if stack is empty
            IF stack.isEmpty():
                RETURN FALSE  // No matching opening bracket

            // Pop and check if it matches
            top = stack.pop()
            IF NOT isMatchingPair(top, ch):
                RETURN FALSE  // Mismatched pair

    // If stack is empty, all brackets matched
    RETURN stack.isEmpty()

MAIN:
    INPUT expression
    IF isBalanced(expression):
        PRINT "Balanced"
    ELSE:
        PRINT "Not Balanced"
```

---

## C++ Code

```cpp
#include <iostream>
#include <stack>
#include <string>
using namespace std;

class ParenthesesChecker_agb {
private:
    bool isMatchingPair_agb(char opening, char closing) {
        if (opening == '(' && closing == ')')
            return true;
        else if (opening == '{' && closing == '}')
            return true;
        else if (opening == '[' && closing == ']')
            return true;
        return false;
    }

    bool isOpeningBracket_agb(char ch) {
        return (ch == '(' || ch == '{' || ch == '[');
    }

    bool isClosingBracket_agb(char ch) {
        return (ch == ')' || ch == '}' || ch == ']');
    }

public:
    bool isBalanced_agb(string expr) {
        stack<char> st;

        for (int i = 0; i < expr.length(); i++) {
            char ch = expr[i];

            if (isOpeningBracket_agb(ch)) {
                st.push(ch);
            }
            else if (isClosingBracket_agb(ch)) {
                if (st.empty()) {
                    return false;
                }

                char top = st.top();
                st.pop();

                if (!isMatchingPair_agb(top, ch)) {
                    return false;
                }
            }
        }

        return st.empty();
    }

    bool isBalancedWithSteps_agb(string expr) {
        stack<char> st;
        bool balanced = true;

        cout << "\n--- Step-by-Step Analysis ---" << endl;
        cout << "Expression: " << expr << endl;
        cout << "\nStep | Character | Action           | Stack State" << endl;
        cout << "-----+-----------+------------------+-------------" << endl;

        for (int i = 0; i < expr.length(); i++) {
            char ch = expr[i];

            cout << " " << (i+1) << "    |      " << ch << "     | ";

            if (isOpeningBracket_agb(ch)) {
                st.push(ch);
                cout << "Push             | ";
                displayStack_agb(st);
            }
            else if (isClosingBracket_agb(ch)) {
                if (st.empty()) {
                    cout << "ERROR: No match  | Empty" << endl;
                    balanced = false;
                    break;
                }

                char top = st.top();
                st.pop();

                if (isMatchingPair_agb(top, ch)) {
                    cout << "Pop & Match      | ";
                    displayStack_agb(st);
                } else {
                    cout << "ERROR: Mismatch  | ";
                    displayStack_agb(st);
                    balanced = false;
                    break;
                }
            } else {
                cout << "Ignore           | ";
                displayStack_agb(st);
            }
        }

        cout << "\nFinal Stack: ";
        if (st.empty() && balanced) {
            cout << "Empty" << endl;
            return true;
        } else if (balanced && !st.empty()) {
             displayStack_agb(st);
             return false;
        } else {
             return false;
        }
    }

    void displayStack_agb(stack<char> st) {
        if (st.empty()) {
            cout << "Empty" << endl;
            return;
        }

        stack<char> temp;
        while (!st.empty()) {
            temp.push(st.top());
            st.pop();
        }

        while (!temp.empty()) {
            cout << temp.top();
            temp.pop();
            if (!temp.empty()) cout << ", ";
        }
        cout << endl;
    }
};

int main_agb() {
    ParenthesesChecker_agb checker_agb;
    string expression;
    int choice;

    do {
        cout << "\n===== Balanced Parentheses Checker =====" << endl;
        cout << "1. Quick check" << endl;
        cout << "2. Check with step-by-step analysis" << endl;
        cout << "3. Test predefined examples" << endl;
        cout << "4. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;
        cin.ignore();

        switch (choice) {
            case 1:
                cout << "Enter expression: ";
                getline(cin, expression);
                if (checker_agb.isBalanced_agb(expression)) {
                    cout << "Result: BALANCED ✓" << endl;
                } else {
                    cout << "Result: NOT BALANCED ✗" << endl;
                }
                break;

            case 2:
                cout << "Enter expression: ";
                getline(cin, expression);
                if (checker_agb.isBalancedWithSteps_agb(expression)) {
                    cout << "\n✓ Expression is BALANCED!" << endl;
                } else {
                    cout << "\n✗ Expression is NOT BALANCED!" << endl;
                }
                break;

            case 3:
                {
                    string examples[] = {
                        "{[()]}",
                        "{[(])}",
                        "(()",
                        "()[]{}",
                        "({[}])",
                        "[{()}]",
                        "((()))",
                        "((())",
                        "}{",
                        ""
                    };

                    cout << "\n--- Testing Predefined Examples ---" << endl;
                    for (int i = 0; i < 10; i++) {
                        cout << i+1 << ". \"" << examples[i] << "\" → ";
                        if (checker_agb.isBalanced_agb(examples[i])) {
                            cout << "BALANCED ✓" << endl;
                        } else {
                            cout << "NOT BALANCED ✗" << endl;
                        }
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

### Example 1: Balanced Expression

```
===== Balanced Parentheses Checker =====
1. Quick check
2. Check with step-by-step analysis
3. Test predefined examples
4. Exit
Enter your choice: 2
Enter expression: {[()]}

--- Step-by-Step Analysis ---
Expression: {[()]}

Step | Character | Action           | Stack State
-----+-----------+------------------+-------------
 1   |     {     | Push             | {
 2   |     [     | Push             | {, [
 3   |     (     | Push             | {, [, (
 4   |     )     | Pop & Match      | {, [
 5   |     ]     | Pop & Match      | {
 6   |     }     | Pop & Match      | Empty

Final Stack: Empty

✓ Expression is BALANCED!
```

### Example 2: Unbalanced Expression (Mismatched)

```
Enter your choice: 2
Enter expression: {[(])}

--- Step-by-Step Analysis ---
Expression: {[(])}

Step | Character | Action           | Stack State
-----+-----------+------------------+-------------
 1   |     {     | Push             | {
 2   |     [     | Push             | {, [
 3   |     (     | Push             | {, [, (
 4   |     ]     | ERROR: Mismatch  | {, [

✗ Expression is NOT BALANCED!
```

### Example 3: Unbalanced Expression (Extra Closing)

```
Enter your choice: 2
Enter expression: {[()]}]

--- Step-by-Step Analysis ---
Expression: {[()]}]

Step | Character | Action           | Stack State
-----+-----------+------------------+-------------
 1   |     {     | Push             | {
 2   |     [     | Push             | {, [
 3   |     (     | Push             | {, [, (
 4   |     )     | Pop & Match      | {, [
 5   |     ]     | Pop & Match      | {
 6   |     }     | Pop & Match      | Empty
 7   |     ]     | ERROR: No match  | Empty

✗ Expression is NOT BALANCED!
```

### Example 4: Predefined Test Cases

```
Enter your choice: 3

--- Testing Predefined Examples ---
1. "{[()]}" → BALANCED ✓
2. "{[(])}" → NOT BALANCED ✗
3. "(()" → NOT BALANCED ✗
4. "()[]{}" → BALANCED ✓
5. "({[}])" → NOT BALANCED ✗
6. "[{()}]" → BALANCED ✓
7. "((()))" → BALANCED ✓
8. "((())" → NOT BALANCED ✗
9. "}{" → NOT BALANCED ✗
10. "" → BALANCED ✓
```

---

## Dry Run

### Expression: `{[()]}`

| Step | Character | Action      | Stack     | Explanation                     |
| ---- | --------- | ----------- | --------- | ------------------------------- |
| 1    | `{`       | Push        | `{`       | Opening bracket - push          |
| 2    | `[`       | Push        | `{, [`    | Opening bracket - push          |
| 3    | `(`       | Push        | `{, [, (` | Opening bracket - push          |
| 4    | `)`       | Pop & Match | `{, [`    | Closing `)` matches opening `(` |
| 5    | `]`       | Pop & Match | `{`       | Closing `]` matches opening `[` |
| 6    | `}`       | Pop & Match | Empty     | Closing `}` matches opening `{` |

**Result**: Stack is empty → **BALANCED** ✓

---

### Expression: `{[(])}`

| Step | Character | Action   | Stack     | Explanation                            |
| ---- | --------- | -------- | --------- | -------------------------------------- |
| 1    | `{`       | Push     | `{`       | Opening bracket - push                 |
| 2    | `[`       | Push     | `{, [`    | Opening bracket - push                 |
| 3    | `(`       | Push     | `{, [, (` | Opening bracket - push                 |
| 4    | `]`       | Mismatch | -         | Closing `]` does NOT match opening `(` |

**Result**: Mismatch detected → **NOT BALANCED** ✗

---

### Expression: `(()`

| Step | Character | Action      | Stack  | Explanation                     |
| ---- | --------- | ----------- | ------ | ------------------------------- |
| 1    | `(`       | Push        | `(`    | Opening bracket - push          |
| 2    | `(`       | Push        | `(, (` | Opening bracket - push          |
| 3    | `)`       | Pop & Match | `(`    | Closing `)` matches opening `(` |

**Result**: Stack is NOT empty (contains `(`) → **NOT BALANCED** ✗

---

### Expression: `)()`

| Step | Character | Action | Stack | Explanation                                   |
| ---- | --------- | ------ | ----- | --------------------------------------------- |
| 1    | `)`       | Error  | Empty | Closing bracket with no opening - stack empty |

**Result**: No matching opening bracket → **NOT BALANCED** ✗

---

## Conclusion

This program successfully implements a **Balanced Parentheses Checker** using a stack with the following key achievements:

1. **Algorithm Implementation**:

   - Uses stack to track opening brackets
   - Matches each closing bracket with most recent opening bracket
   - Handles three types of brackets: `()`, `{}`, `[]`

2. **Validation Rules**:

   - **Rule 1**: Every opening bracket must have a corresponding closing bracket
   - **Rule 2**: Brackets must be closed in the correct order (LIFO)
   - **Rule 3**: Types must match (can't close `(` with `]`)

3. **Detection Cases**:

   - **Balanced**: `{[()]}`, `()[]{}`, `((()))`
   - **Unbalanced - Mismatch**: `{[(])}`, `({[}])`
   - **Unbalanced - Missing opening**: `}()`, `]`
   - **Unbalanced - Missing closing**: `((()`, `{[(`
   - **Unbalanced - Wrong order**: `)(`, `}{`

4. **Core Logic**:

   ```
   For each character:
     - Opening bracket → Push to stack
     - Closing bracket → Pop and check match
     - If mismatch or stack empty → Not balanced

   After processing:
     - Stack empty → Balanced
     - Stack not empty → Not balanced
   ```

5. **Features**:
   - Step-by-step visualization
   - Multiple test cases
   - Clear error detection
   - Stack state display

**Time Complexity**: O(n) where n is length of expression  
**Space Complexity**: O(n) for stack in worst case (all opening brackets)  
**Applications**:

- Compiler syntax checking
- Code editor bracket matching
- Expression validation
- XML/HTML tag validation
