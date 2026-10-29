## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

## AIM:
 

 

To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




Program:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

char a[5][5];

void generate(char key[]) {
    int used[26] = {0}, k = 0;
    char s[100];

    strcpy(s, key);
    strcat(s, "abcdefghiklmnopqrstuvwxyz");
    used['j' - 'a'] = 1;

    for (int i = 0; s[i]; i++) {
        char c = tolower((unsigned char)s[i]);

        if (c < 'a' || c > 'z')
            continue;

        if (c == 'j')
            c = 'i';

        if (!used[c - 'a']) {
            a[k / 5][k % 5] = c;
            used[c - 'a'] = 1;
            k++;
        }
    }
}

void find(char c, int *r, int *col) {
    if (c == 'j')
        c = 'i';

    for (int i = 0; i < 5; i++)
        for (int j = 0; j < 5; j++)
            if (a[i][j] == c) {
                *r = i;
                *col = j;
                return;
            }
}

int main() {
    char key[100], text[100], p[200], out[200];
    int n = 0, m = 0;

    printf("Enter keyword: ");
    scanf("%99s", key);

    printf("Enter plaintext: ");
    scanf("%99s", text);

    for (int i = 0; text[i]; i++) {
        char c = tolower((unsigned char)text[i]);

        if (c >= 'a' && c <= 'z')
            p[n++] = (c == 'j') ? 'i' : c;
    }

    generate(key);

    for (int i = 0; i < n;) {
        char x = p[i], y;

        if (i + 1 >= n || p[i] == p[i + 1]) {
            y = 'x';
            i++;
        } else {
            y = p[i + 1];
            i += 2;
        }

        int r1, c1, r2, c2;
        find(x, &r1, &c1);
        find(y, &r2, &c2);

        if (r1 == r2) {
            c1 = (c1 + 1) % 5;
            c2 = (c2 + 1) % 5;
        } else if (c1 == c2) {
            r1 = (r1 + 1) % 5;
            r2 = (r2 + 1) % 5;
        } else {
            int t = c1;
            c1 = c2;
            c2 = t;
        }

        out[m++] = a[r1][c1];
        out[m++] = a[r2][c2];
    }

    out[m] = '\0';
    printf("Cipher text: %s\n", out);

    return 0;
}
```
Output:
```
Enter keyword: crypto
Enter plaintext: kiruthika
Cipher text: zamoqw
```
Result:
Thus, the Playfair cipher encryption technique was implemented successfully using C language.
