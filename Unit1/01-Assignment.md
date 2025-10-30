# Assignment 1: Basic String Operations (Length, Copy, Reverse, Concatenation)


## Pseudocode
1. **Find Length**
   - Initialize `counter = 0`
   - While character != `'\0'`, increment counter
   - Return counter

2. **Copy String**
   - Copy each character from source to destination
   - Append `'\0'` at the end

3. **Reverse String**
   - Find length
   - Copy characters from end to beginning

4. **Concatenate Strings**
   - Copy first string to result
   - Append second string to result

5. **Main Function**
   - Input two strings
   - Perform length, copy, reverse, and concatenate
   - Display all results


## Code (C++)
```cpp
#include <iostream>
using namespace std;

int length_agb(char str[]) {
    int len = 0;
    while (str[len] != '\0') {
        len++;
    }
    return len;
}

void copy_agb(char src[], char dest[]) {
    int i = 0;
    while (src[i] != '\0') {
        dest[i] = src[i];
        i++;
    }
    dest[i] = '\0';
}

void reverse_agb(char str[], char rev[]) {
    int len = length_agb(str);
    for (int i = 0; i < len; i++) {
        rev[i] = str[len - i - 1];
    }
    rev[len] = '\0';
}

void concat_agb(char str1[], char str2[], char result[]) {
    int i = 0, j = 0;
    while (str1[i] != '\0') {
        result[j++] = str1[i++];
    }
    i = 0;
    while (str2[i] != '\0') {
        result[j++] = str2[i++];
    }
    result[j] = '\0';
}

int main() {
    char str1[50], str2[50], result[100], rev[50], copied[50];
    cout << "Enter first string: ";
    cin >> str1;
    cout << "Enter second string: ";
    cin >> str2;

    cout << "Length of first string: " << length_agb(str1) << endl;

    copy_agb(str1, copied);
    cout << "Copied string: " << copied << endl;

    reverse_agb(str1, rev);
    cout << "Reversed string: " << rev << endl;

    concat_agb(str1, str2, result);
    cout << "Concatenated string: " << result << endl;

    return 0;
}
```

##  Output
```
Enter first string: hello
Enter second string: world
Length of first string: 5
Copied string: hello
Reversed string: olleh
Concatenated string: helloworld
```

## Dry Run
**Input:**  
`str1 = "hello"`  
`str2 = "world"`

| Step | Operation | Output |
|------|------------|---------|
| 1 | length("hello") | 5 |
| 2 | copy("hello") | "hello" |
| 3 | reverse("hello") | "olleh" |
| 4 | concat("hello", "world") | "helloworld" |


