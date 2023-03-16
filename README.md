[assignment1 (2).pdf](https://github.com/NotAProPilot/PR2-Assignment/files/10964434/assignment1.2.pdf)
# PR2-Assignment
Like the title, this is PR2 Assignment.

## Update 1 (After panicking about TextIO)
(According to ChatGPT): TextIO can be considered an alternative to Scanner in Java. Both TextIO and Scanner provide methods for reading and writing text, numbers, and other data types from various input/output sources, such as files, console, and strings.

## Update 2: Detailed breakdown of the code (Non - Javadoc compliant)
So, these line:
```java
 private static final char GREEN = 'G';
    /** constant value for the blue bean*/
    private static final char BLUE = 'B';
    /** constant for removed beans */
    private static final char REMOVED = '-';
    /** the null character*/
    private static final char NULL = '\u0000';
```
Essentially mean that whenever we type GREEN/BLUE/NULL in our code, it'll return G,B.

Meanwhile, these line:
```java
 char[][] tins = {
                {BLUE, BLUE, BLUE, GREEN, GREEN},
                {BLUE, BLUE, BLUE, GREEN, GREEN, GREEN},
                {GREEN},
                {BLUE},
                {BLUE, GREEN}
        };
```
This code is initializing a 2-dimensional array of characters called tins. The array tins is an array of arrays, where each sub-array represents a tin of beans. Each sub-array is made up of individual beans represented by characters, where 'B' stands for blue bean, 'G' stands for green bean, and '-' stands for an empty space.

So for example, the first sub-array in tins looks like this:

```java
{BLUE, BLUE, BLUE, GREEN, GREEN}
```

which means that this tin contains 3 blue beans, 2 green beans, and no empty spaces. This array of tins will be used to test the tinGame function in the main method.
