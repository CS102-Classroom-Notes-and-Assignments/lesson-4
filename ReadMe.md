# lesson-4

## While Loops
- Continues to run until the expression inside the while is terminated
- Commonly used with some form of counter

```c
expr1;
while (expr2)
{
  statement
  expr3;
}
```

### While Loop Example

- Note that fahr increases by 20 per iteration.
- /*    */ denotes a multi line comment, // is a single line comment.
- Formatted printing:
	- printf(“%3d %6d\n”, fahr, celsius) to print the first number of each line in a field three digits wide, and the second in a field 6 digits wide.
	- printf(%6.2f, 45.2342) → print as floating point, at least 6 wide and 2 after decimal point


```c
#include <stdio.h>

/* K&R pg. 11
 * print Fahrenheit-Celsius table 
 * for fahr = 0, 20, ..., 300 */

int main()
{
    float fahr, celsius;
    int lower, upper, step;

    lower = 0; /* lower limit of temperature table */
    upper = 300; /* upper limit */
    step = 20; /* step size*/

    fahr = lower;
    while (fahr <= upper)
    {
        celsius = 5.0 * (fahr-32.0) / 9.0;
        printf("%3.0f %6.1f\n", fahr, celsius);
        fahr = fahr + step;
    }
    return 0;
}
```


### Infinite Loop
```c
#include <stdio.h>

int main()
{
    int i = 0;
    while (i >= 0)
    {
        printf("This is an infinite loop! (or is it?) i=%d\n", i);
        i++;
    }
    return 0;
}
```
How will you make the above loop truly infinite?

## For Loops
Similar to while, except in one line

Order of evaluation:
1. initialization with expr1
2. if expr2 is true continue to statement, if not, loop ends
3. do expr3
4. goto step 2


```
for (expr1; expr2; expr3)
    statement
```
A typical expr3 is i++

Any of the three expressions can be omitted, although the semicoloons must remain. Example: 
```
for (;;) { ..}
```
is an “infinite” loop, which can be broken by break or return

** page 57 has the shellsort algorithm written with 3 for loops - it's a good complex example

### For Loops Example
```c
#include <stdio.h>

// K&R pg. 13
// Print Fahrenheit-Celsius table
int main()
{
    int fahr;
    for (fahr = 0; fahr <= 300; fahr += 20)
    {
        printf("%3d %6.1f\n", fahr, (5.0/9.0)*(fahr-32));
    }

    return 0;
}
```
One final C operator is the **comma** which most often is used in the for statement. A pair of expressions separated by a comma is evaluated left to right, and the type and value of the result are the type and value of the right operand. Thus in a for statement, it is possible to place multiple expressions in the various parts, for example to process two indices in parallel. 

Comma operators should be used sparingly. The most suitable uses are for constructs strongly related to each other. The commas that separate function arguments, variables in declarations are NOT comma operators. 

#### Example for string reverse:
```c
#include <stdio.h>
#include <string.h>

int main()
{
    char s[] = "hello";
    char c;
    for (int i = 0, j = strlen(s) - 1; i < j; i++, j--)
    {
        c = s[i], s[i] = s[j], s[j] = c;
    }

    printf("%s\n", s);
    return 0;
}
```

## Symbolic constants:
```c
#include <stdio.h>
#define LOWER 0 		/* lower limit of table */
#define UPPER 300		/* upper limit */
#define STEP 20		/* step size */

// K&R pg. 13
// Print Fahrenheit-Celsius table
int main()
{
    int fahr;
    for (fahr = LOWER; fahr <= UPPER; fahr += STEP)
    {
        printf("%3d %6.1f\n", fahr, (5.0/9.0)*(fahr-32));
    }

    return 0;
}
```

## Basic Arrays
DECLARING AND ACCESSING ARRAYS

```c
#include <stdio.h>

int main()
{
    int intArr[5];
    intArr[0] = 0;
    intArr[1] = 1;
    intArr[2] = 2;
    intArr[3] = 3;
    intArr[4] = 4;
    printf("%d %d %d %d %d\n",
           intArr[0], intArr[1], intArr[2],
           intArr[3], intArr[4]);

    char charArr[3] = {'A', 'B', 'C'};
    for (int ii = 0; ii < 3; ii++)
    {
        printf("%c ", charArr[ii]);
    }
    charArr[0] = 'Z';
    printf("\n");
    for (int ii = 0; ii < 3; ii++)
    {
        printf("%c ", charArr[ii]);
    }
    return 0;
}
```

## Count Digits, White Space, and Others
Run the program against itself with ./a.exe < count.c
Array is defined as int ndigit[10];

```c
#include <stdio.h>

// K&R pg. 22
// count digits, white space, others

int main()
{
    int c, i, nwhite, nother;
    int ndigit[10];

    nwhite = nother = 0;
    for (i = 0; i < 10; ++i)
    {
        ndigit[i] = 0;
    }

    while ((c = getchar()) != EOF)
    {
        if (c >= '0' && c <= '9')
            ++ndigit[c-'0'];
        else if (c == ' ' || c == '\n' || c == '\t')
            ++nwhite;
        else
            ++nother;
    }

    printf("digits =");
    for (i = 0; i < 10; ++i)
        printf(" %d", ndigit[i]);
    printf(", white space = %d, other = %d\n",
        nwhite, nother);

    return 0;
}
```

## AtoI
```c
#include <stdio.h>
#include <ctype.h>

// K&R pg. 61
// atoi: convert s to integer; version 1
int main()
{
    char s[] = "-10";

    int i, n, sign;

    for (i = 0; isspace(s[i]); i++) // skip white space
        ;
    sign = (s[i] == '-') ? -1 : 1;
    if (s[i] == '+' || s[i] == '-') // skip sign
        i++;
    for (n = 0; isdigit(s[i]); i++)
        n = 10 * n + (s[i] - '0');

    int result = sign * n;
    printf("%d", result);
    return 0;
}
```
Note that the functionality is living in main.
Note that a lot of curly braces are not necessary.
How would you modify this to use user input using

## AtoI With getchar()
```c
#include <stdio.h>
#include <ctype.h>

// K&R pg. 61
// atoi: convert s to integer; version 1
int main()
{
    char s[5]; // needs a const
    char c = getchar();
    int cidx = 0;
    while (c != '\n')
    {
        s[cidx] = c;
        c = getchar();
        cidx++;
    }


    int i, n, sign;

    for (i = 0; isspace(s[i]); i++) // skip white space
        ;
    sign = (s[i] == '-') ? -1 : 1;
    if (s[i] == '+' || s[i] == '-') // skip sign
        i++;
    for (n = 0; isdigit(s[i]); i++)
        n = 10 * n + (s[i] - '0');

    int result = sign * n;
    printf("%d", result);
    return 0;
}
```

## Shell Sort
Note the nested loops.
You will learn more about sorts in DSA I.

```c
#include <stdio.h>

// K&R pg. 62
// shellsort: sort v[0]...v[n-1] into increasing order
int main()
{
    int v[10] = {4, 6, 2, 1, 3, 5, 7, 9, 8, 0};
    int n = 10;
    int gap, i, j, temp;
    for (gap = n/2; gap > 0; gap /= 2)
        for (i = gap; i < n; i++)
            for (j=i-gap; j>=0 && v[j]>v[j+gap]; j-=gap)
            {
                temp = v[j];
                v[j] = v[j+gap];
                v[j+gap] = temp;
            }

    printf("Sorted: ");
    for (int ii=0; ii<n; ii++)
        printf("%d ", v[ii]);

    return 0;
}
```

## Reverse
Note the complex expressions in the for loop.
Strings are just arrays of chars!

```c
#include <stdio.h>
#include <string.h>

// K&R pg. 62
// reverse: reverse string s in place

int main()
{
    char s[] = "ECE160";

    int c, i, j;
    for (i=0, j=strlen(s)-1; i<j; i++, j--)
    {
        c = s[i];
        s[i] = s[j];
        s[j] = c;
    }
    printf("%s", s);
    return 0;
}
```

## Do While
In contrast to the while and for loop, the body is executed first, and then the condition is tested. The do will always execute first, and is executed at least once.
This loop will end when the while part is not satisfied.
```
do
    statement
while (expression);
```

DO WHILE EXAMPLE
The do-while is necessary, ar at least convenient, since at least one character must be installed in the array s, even if is zero.
The job of itoa is slightly more complicated than might be thought at first, because the easy methods of generating the digits generate them in the wrong order. So we have chosen to generate the string backwards, then reverse it.
```c
#include <stdio.h>
#include <string.h>

// K&R pg. 64
// itoa: convert n to characters in s

int main()
{
    int n = -77;
    char s[10];

    int idx, sign;
    if ((sign = n) < 0) // record sign
        n = -n;         // make n positive
    idx = 0;
    do {
        s[idx++] = n % 10 + '0';
    } while ((n /= 10) > 0);
    if (sign < 0)
        s[idx++] = '-';
    s[idx] = '\0';

    // reverse s
    int c, i, j;
    for (i=0, j=strlen(s)-1; i<j; i++, j--)
    {
        c = s[i];
        s[i] = s[j];
        s[j] = c;
    }
    printf("%s", s);
    return 0;
}
```

## Break & Continue
break -- allows you to exit the innermost enclosing loop immediately
Can be used in a for, while, or do loop
We have seen this used in switch
continue -- allows the next iteration of the enclosing loop to begin
Can be used in a for, while, or do loop
In the while and do this means that the test part is executed immediately, in the for loop, control passes to the increment step.
Continue does not apply to switch.  A continue inside a switch inside a loop causes the next loop iteration.

## Break Example
```c
#include <stdio.h>
#include <string.h>

// K&R pg. 65
// trim: remove trailing blanks, tabs, newlines
int main()
{
    char s[] = "HELLO   ";
    int n;
    for (n = strlen(s)-1; n >= 0; n--)
        if (s[n] != ' ' && s[n] != '\t' && s[n] != '\n')
            break;
    s[n+1] = '\0';
    printf("%d\n", n);
    printf("%s.", s);

    return 0;
}
```

## Continue Example
```c
#include <stdio.h>

// skip printing a number with continue

int main()
{
    for (int ii=0; ii<=10; ii++)
    {
        if (ii==7)
            continue;
        printf("%d ", ii);
    }

    return 0;
}
```

## GOTO
DO NOT USE, this can usually be replaced with other more readable code
Allows you to go to other lines in your program directly
The one situation where it might be helpful is if you have multiple nested loops, and you want to exit out of all the loops. Goto can do this with one statement, instead of using many breaks as breaks only exit out of the innermost loop.
For this reason we will skip an example on goto.


## Input & Output
### SCANF
Don't worry about what the syntax means for now
```c
#include <stdio.h>

// K&R pg. 158
// rudimentary calculator

int main()
{
    double sum, v;
    sum = 0;
    while (scanf("%lf", &v) == 1)
        printf("\t%.2f\n", sum+=v);

    return 0;
}
```

## Basic Functions
### Arguments & Return Values
Functions splits up the main function to more readable chunks
Generally, you don't want functions to be too large (<100 lines)
Functions are in the form:
```
return-type function-name(parameter declarations, if any)
{
  declarations
  statements
  return
}
```

## Example of Using a Function
A function must be defined in the file that is used. A function definition looks like: return-type function-name(parameter declarations, if any)

```
{ declarations 
statements}
```

The function declaration, called a function prototype, has to match with the function definition. It is an error if the definition of a function or any uses of it doe not agree with its prototype.

Parameter names do not have to match, and are in fact optional. The prototype for a power() function can be written as int power(int m, int n); Or int power(int, int). Later in the file the function will be defined as 
```
int power(int base, int n) {...}
```
The function must be declared and or defined before it is used in main.

The variables used in power are local to the function, the i in power() is not related to the i in main(). These local variables are also referred to as automatic variables. 

Just as the power function returns a value to the main function, the main function returns a value to its caller, which in effect is the environment in which the program was executed. Typically a return value of zero implies normal termination; non-zero values signal unusual or erroneous termination conditions.

```c
#include <stdio.h>

// K&R pg. 24-25
// power function

// Function prototype at the beginning
int power(int m, int n);

int main()
{
    int i;
    for (i=0; i<10; ++i)
        printf("%d %d %d\n", i, power(2,i), power(-3,i));
    return 0;
}

// power: raise base to n-th power; n >= 0
int power(int base, int n)
{
    int i, p;
    p = 1;
    for (i=1; i<=n; ++i)
        p = p * base;
    return p;
}
```
It is good to use functions - to make the code more readable, re-usability of functions, …


The alternative to local variables are external variables. These are variables that are external to all functions, that is variables that can be accessed by name by any function. They are globally accessible. 

The external variable must be defined, exactly once, outside of any function; this sets aside storage for it. The variable must also be declared in each function that wants to access it; this states the type of the variable.

The declaration may be an explicit extern statement or may be implicit from context. The extern declaration can be omitted if the definition of the external variable occurs in the source file before its use in a particular function. In fact, its common practice to place definitions of all external variables at the beginning of the source file, and then omit all extern declarations. The extern declarations below are redundant.

If the program is in several source files, and a variable is defined in file1 and used in file2 and file3, then extern declarations are needed in file2 and file3 to connect the occurrences of the variable. The usual practice is to collect extern declarations of variables and functions in a separate file, historically called a header, that is included by #include at the front of each source file. The suffix .h is convention for header files.

Definition = place where the variable is created or assigned storage
Declaration = place where the nature of the variable is stated but no storage is allocated

```c
#include <stdio.h>
#define MAXLINE 1000

// external variables are declared hear with their type, so storage is allocated for them
int max;
char line[MAXLINE];
char longest[MAXLINE];

int getline(void);
void copy(void);

main(){
	int len;
	extern int max;
	extern char longest[];

	max = 0;
while (( len = getline()) > 0)
	if (len > max) {
		max = len;
		copy();
	}
	if (max > 0)
		printf(“%s”, longest)
	Return 0;
}

int getline(void)
{ 
	int c, i;
	extern char line[];
	
	for (i=0; i < MAXLINE -1 && (c = getchar() != EOF && c != ‘\n’; ++i)
		line[i] = c;
	if (c == ‘\n’) {
		line[i] = c;
		++i;
	}
	line[i] = ‘\0’;
	return i;
}

void copy(void)
{
	int i;
	extern char line[], longest[];
	
	i = 0;
	while((longest[i] = line[i]) != ‘\0’)
		++i;
}
	
```

 
# Homework:
- The C Programming Language, 2nd Edition - Kerninghan & Ritchie, Chapter 1.2, 1.3, 1.5, 1.6, 1.7, 3.4, 3.5, 3.6, 3.7, 3.8, 4.1, 4.2
- hw link
