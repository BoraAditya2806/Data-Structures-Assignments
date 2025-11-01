# Assignment 7

## Problem Statement
Write a C++ program to represent and perform **addition of two polynomials** using **Singly Linked List**.  
Each term of a polynomial consists of a coefficient and an exponent. The program should create two linked lists representing two polynomials and perform their addition to produce a resultant polynomial.

## Pseudocode

1. Define a structure `Node` containing:
   - `coeff` → coefficient of term
   - `pow` → power of variable
   - `next` → pointer to next node
2. Create functions to:
   - Insert a term into the polynomial list in decreasing order of power.
   - Display a polynomial in readable format.
   - Add two polynomial lists term by term.
3. Traverse both polynomial lists simultaneously:
   - If powers are equal, add coefficients and store result.
   - If one power is greater, copy that term to the result.
4. Display the resulting polynomial.

## Code (C++)
```cpp
#include <iostream>
using namespace std;

struct Node {
    int coeff;
    int pow;
    Node *next;
};

Node* createNode_agb(int coeff, int pow) {
    Node *temp = new Node;
    temp->coeff = coeff;
    temp->pow = pow;
    temp->next = nullptr;
    return temp;
}

void insertTerm_agb(Node* &poly, int coeff, int pow) {
    Node *newNode = createNode_agb(coeff, pow);
    if (!poly || poly->pow < pow) {
        newNode->next = poly;
        poly = newNode;
        return;
    }
    Node *temp = poly;
    while (temp->next && temp->next->pow >= pow)
        temp = temp->next;
    newNode->next = temp->next;
    temp->next = newNode;
}

void display_agb(Node *poly) {
    Node *temp = poly;
    while (temp) {
        cout << temp->coeff << "x^" << temp->pow;
        if (temp->next && temp->next->coeff >= 0) cout << " + ";
        temp = temp->next;
    }
    cout << endl;
}

Node* addPoly_agb(Node *poly1, Node *poly2) {
    Node *result = nullptr, *tail = nullptr;

    while (poly1 && poly2) {
        int coeff, pow;
        if (poly1->pow == poly2->pow) {
            coeff = poly1->coeff + poly2->coeff;
            pow = poly1->pow;
            poly1 = poly1->next;
            poly2 = poly2->next;
        } else if (poly1->pow > poly2->pow) {
            coeff = poly1->coeff;
            pow = poly1->pow;
            poly1 = poly1->next;
        } else {
            coeff = poly2->coeff;
            pow = poly2->pow;
            poly2 = poly2->next;
        }

        if (coeff != 0) {
            Node *newNode = createNode_agb(coeff, pow);
            if (!result) result = tail = newNode;
            else {
                tail->next = newNode;
                tail = newNode;
            }
        }
    }

    while (poly1) {
        Node *newNode = createNode_agb(poly1->coeff, poly1->pow);
        if (!result) result = tail = newNode;
        else {
            tail->next = newNode;
            tail = newNode;
        }
        poly1 = poly1->next;
    }

    while (poly2) {
        Node *newNode = createNode_agb(poly2->coeff, poly2->pow);
        if (!result) result = tail = newNode;
        else {
            tail->next = newNode;
            tail = newNode;
        }
        poly2 = poly2->next;
    }

    return result;
}

int main() {
    Node *poly1 = nullptr, *poly2 = nullptr, *sum = nullptr;

    insertTerm_agb(poly1, 5, 2);
    insertTerm_agb(poly1, 4, 1);
    insertTerm_agb(poly1, 2, 0);

    insertTerm_agb(poly2, 5, 1);
    insertTerm_agb(poly2, 5, 0);

    cout << "First Polynomial: ";
    display_agb(poly1);

    cout << "Second Polynomial: ";
    display_agb(poly2);

    sum = addPoly_agb(poly1, poly2);

    cout << "Resultant Polynomial after Addition: ";
    display_agb(sum);

    return 0;
}
```

## Input
```
First Polynomial: 5x^2 + 4x^1 + 2x^0
Second Polynomial: 5x^1 + 5x^0
```

## Output
```
First Polynomial: 5x^2 + 4x^1 + 2x^0
Second Polynomial: 5x^1 + 5x^0
Resultant Polynomial after Addition: 5x^2 + 9x^1 + 7x^0
```

## Dry Run (Step-by-Step in Tabular Form)

| Step | Poly1 Term | Poly2 Term | Action | Result |
|------|-------------|-------------|----------|----------|
| 1 | 5x² | - | Copy term | 5x² |
| 2 | 4x¹ | 5x¹ | Add → 9x¹ | 5x² + 9x¹ |
| 3 | 2x⁰ | 5x⁰ | Add → 7x⁰ | 5x² + 9x¹ + 7x⁰ |

**Final Result:**  
`5x² + 9x¹ + 7x⁰`

## Conclusion
The program successfully represents and adds two polynomials using a **Singly Linked List**.  
Each term is stored as a node containing the coefficient and exponent, and the addition is performed efficiently by traversing both lists in parallel.
