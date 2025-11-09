# Assignment 2

## Problem Statement
Convert a given infix expression to its postfix form using a stack and show the operations step by step.  
Example expression: `a-b*c-d/e+f`

## Pseudocode
1. Initialize an empty stack for operators and an empty output string (postfix).
2. Scan the infix expression from left to right for each token:
   - If token is an operand (letter or number), append it to output.
   - If token is `'('`, push it onto stack.
   - If token is `')'`, pop and append operators from stack until `'('` is popped.
   - If token is an operator (`+ - * / ^`):
     - While stack is not empty and top of stack has higher or equal precedence (for left-associative) than current operator, pop it to output.
     - Push current operator on stack.
3. After processing all tokens, pop remaining operators to output.
4. The output string is the postfix expression.

Operator precedence (high → low): `^` (right associative), `*` `/` (left), `+` `-` (left).

## Code (C++)
```cpp
#include <iostream>
#include <stack>
#include <string>
using namespace std;

int precedence_agb(char op) {
    if (op == '^') return 3;
    if (op == '*' || op == '/') return 2;
    if (op == '+' || op == '-') return 1;
    return 0;
}

bool isOperator_agb(char c) {
    return c == '+' || c == '-' || c == '*' || c == '/' || c == '^';
}

string infixToPostfix_agb(const string &infix) {
    stack<char> st;
    string output = "";

    for (size_t i = 0; i < infix.length(); ++i) {
        char c = infix[i];
        if (isspace(c)) continue;
        if (isalnum(c)) {
            output.push_back(c);
        } else if (c == '(') {
            st.push(c);
        } else if (c == ')') {
            while (!st.empty() && st.top() != '(') {
                output.push_back(st.top());
                st.pop();
            }
            if (!st.empty()) st.pop();
        } else if (isOperator_agb(c)) {
            while (!st.empty() && (
                (precedence_agb(st.top()) > precedence_agb(c)) ||
                (precedence_agb(st.top()) == precedence_agb(c) && c != '^')
            ) && st.top() != '(') {
                output.push_back(st.top());
                st.pop();
            }
            st.push(c);
        }
    }

    while (!st.empty()) {
        if (st.top() != '(' && st.top() != ')') output.push_back(st.top());
        st.pop();
    }

    return output;
}
```

## Dry Run
Convert: `a - b * c - d / e + f`

We will show token, action, operator stack (top on right), and current postfix output after each step.

| Step | Token | Action | Stack (bottom → top) | Postfix |
|------|-------|--------|----------------------|---------|
| 1 | a | Operand → output | (empty) | a |
| 2 | - | Push '-' | - | a |
| 3 | b | Operand → output | - | ab |
| 4 | * | '*' has higher precedence than '-' → push '*' | - , * | ab |
| 5 | c | Operand → output | - , * | abc |
| 6 | - | Next operator '-' encountered: pop '*' (higher precedence) to output | - | abc* |
| 7 | - | Now compare '-' with top '-' (equal precedence, left-associative): pop previous '-' then push new '-' | - | abc*- |
| 8 | d | Operand → output | - | abc*-d |
| 9 | / | '/' higher than '-' → push '/' | - , / | abc*-d |
| 10 | e | Operand → output | - , / | abc*-de |
| 11 | + | '+' lower than '/' → pop '/' to output; compare '+' with '-' (equal precedence) → pop '-' then push '+' | + | abc*-de/ - becomes abc*-de/-, after popping both we get abc*-de/-, then push '+' => stack: + |
| 12 | f | Operand → output | + | abc*-de/-f |
| 13 | End | Pop remaining '+' | (empty) | abc*-de/-f+ |

Final postfix: `abc*-de/-f+`

## Short Explanation
- Multiplication and division are applied before addition and subtraction because of higher precedence.
- When operators have equal precedence and are left-associative (`+ - * /`), the operator on the stack is popped before pushing the new operator.
- The caret `^` is treated as right-associative and handled accordingly in the algorithm.


## Conclusion
The algorithm uses a stack to manage operators and their precedence and produces the correct postfix (Reverse Polish) notation. The step-by-step table helps in visualizing the intermediate stack state and output at each token.
