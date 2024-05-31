The repository for Assignment 1, PR2 class, Hanoi University
[assignment1 (2).pdf](https://github.com/NotAProPilot/PR2-Assignment/files/10964434/assignment1.2.pdf)
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


#Temp code (as of 2PM, March 16 '23)
```java
package a1_2101040150;

import java.util.Arrays;
import java.util.Random;
import textio.TextIO;

public class CoffeeTinGame {
    /** constant value for the green bean*/
    private static final char GREEN = 'G';
    /** constant value for the blue bean*/
    private static final char BLUE = 'B';
    /** constant for removed beans */
    private static final char REMOVED = '-';
    /** the null character*/
    private static final char NULL = '\u0000';

    /**
     * the main procedure
     * @effects
     *    initialise a coffee tin
     *    {@link TextIO#putf(String, Object...)}: print the tin content
     *    {@link @tinGame(char[])}: perform the coffee tin game on tin
     *    {@link TextIO#putf(String, Object...)}: print the tin content again
     *    if last bean is correct
     *      {@link TextIO#putf(String, Object...)}: print its colour
     *    else
     *      {@link TextIO#putf(String, Object...)}: print an error message
     */
    @SuppressWarnings("JavadocReference")
    public static void main(String[] args) {
        // initialise some beans
        char[][] tins = {
                {BLUE, BLUE, BLUE, GREEN, GREEN},
                {BLUE, BLUE, BLUE, GREEN, GREEN, GREEN},
                {GREEN},
                {BLUE},
                {BLUE, GREEN}
        };

        for (int i = 0; i < tins.length; i++) {
            char[] tin = tins[i];

            // expected last bean
            // p0 = green parity /\
            // (p0=1 -> last=GREEN) /\ (p0=0 -> last=BLUE)
            // count number of greens
            int greens = 0;
            for (char bean : tin) {
                if (bean == GREEN)
                    greens++;
            }
            // expected last bean
            final char last = (greens % 2 == 1) ? GREEN : BLUE;

            // print the content of tin before the game
            System.out.printf("%nTIN (%d Gs): %s %n", greens, Arrays.toString(tin));

            // perform the game
            // get actual last bean
            char lastBean = tinGame(tin);
            // lastBean = last \/ lastBean != last

            // print the content of tin and last bean
            System.out.printf("tin after: %s %n", Arrays.toString(tin));

            // check if last bean as expected and print
            if (lastBean == last) {
                System.out.printf("last bean: %c%n", lastBean);
            } else {
                System.out.printf("Oops, wrong last bean: %c (expected: %c)%n", lastBean, last);
            }
        }
    }
    private static final String[] BeansBag = new String[30];
    static {
        int blueCount = 0;
        int greenCount = 0;
        int spaceCount = 0;
        for (int i = 0; i < BeansBag.length; i++) {
            if (blueCount < BeansBag.length / 3) {
                BeansBag[i] = "blue";
                blueCount++;
            } else if (greenCount < BeansBag.length / 3) {
                BeansBag[i] = "green";
                greenCount++;
            } else {
                BeansBag[i] = "available";
                spaceCount++;
            }
        }
        System.out.println("BeansBag: blue=" + blueCount + ", green=" + greenCount + ", available=" + spaceCount);
    }
    /**

     Generates a random integer between 1 and n (inclusive).
     @param n the maximum value of the generated integer (exclusive)
     @return a random integer between 1 and n (inclusive)
     */
    public static int randInt(int n) {
        Random rand = new Random();
        return rand.nextInt(n);
    }
    /**
     * Performs the coffee tin game to determine the colour of the last bean
     *
     * @requires tin is not null /\ tin.length > 0
     * @modifies tin
     * @effects <pre>
     *   take out two beans from tin
     *   if same colour
     *     throw both away, put one blue bean back
     *   else
     *     put green bean back
     *   let p0 = initial number of green beans
     *   if p0 = 1
     *     result = `G'
     *   else
     *     result = `B'
     *   </pre>
     */
    public static char tinGame(char[] tin) {
        while (hasAtLeastTwoBeans(tin)) {
            // take two beans from tin
            char[] twoBeans = takeTwo(tin);
            // process beans to update tin
            char b1 = twoBeans[0];
            char b2 = twoBeans[1];
            // process beans to update tin
            if (b1 == b2) {
                // put B in bin
                putIn(tin, BLUE);
            } else { // BG, GB
                // put G in bin
                putIn(tin, GREEN);
            }
        }
        return anyBean(tin);
    }

    /**
     * @effects
     *  if tin has at least two beans
     *    return true
     *  else
     *    return false
     */
    private static boolean hasAtLeastTwoBeans(char[] tin) {
        int count = 0;
        for (char bean : tin) {
            if (bean != REMOVED) {
                count++;
            }

            if (count >= 2) // enough beans
                return true;
        }

        // not enough beans
        return false;
    }

    /**
     * @requires tin has at least 2 beans left
     * @modifies tin
     * @effects
     *  remove any two beans from tin and return them
     */
    private static char[] takeTwo(char[] tin) {
        char first = takeOne(tin);
        char second = takeOne(tin);

        return new char[]{first, second};
    }

    /**
     * @requires tin has at least one bean
     * @modifies tin
     * @effects
     *   remove any bean from tin and return it
     */
    public static char takeOne(char[] tin) {
        int n = tin.length;
        int randomIndex = randInt().applyAsInt(n);
        char bean = ' ';
        if (randomIndex >= 0 && randomIndex < n) {
            bean = tin[randomIndex];
            if (bean != REMOVED) {
                tin[randomIndex] = REMOVED;
            } else {
                // try to find another bean
                for (int i = 0; i < n; i++) {
                    bean = tin[i];
                    if (bean != REMOVED) {
                        tin[i] = REMOVED;
                        return bean;
                    }
                }
                bean = ' ';
            }
        }
        return bean;
        }

        // no beans left
        return NULL;
    }

    /**
     * @requires tin has vacant positions for new beans
     * @modifies tin
     * @effects
     *   place bean into any vacant position in tin
     */
    private static void putIn(char[] tin, char bean) {
        for (int i = 0; i < tin.length; i++) {
            if (tin[i] == REMOVED) { // vacant position
                tin[i] = bean;
                break;
            }
        }
    }

    /**
     * @effects
     *  if there are beans in tin
     *    return any such bean
     *  else
     *    return '\u0000' (null character)
     */
    private static char anyBean(char[] tin) {
        for (char bean : tin) {
            if (bean != REMOVED) {
                return bean;
            }
        }

        // no beans left
        return NULL;
    }
}
// Update 1 (1.08 PM, March 8 23): Remove the utils.IO line, return the following:
/*
TIN (2 Gs): [B, B, B, G, G]
tin after: [B, -, -, -, -]
last bean: B

TIN (3 Gs): [B, B, B, G, G, G]
tin after: [G, -, -, -, -, -]
last bean: G

TIN (1 Gs): [G]
tin after: [G]
last bean: G

TIN (0 Gs): [B]
tin after: [B]
last bean: B

TIN (1 Gs): [B, G]
tin after: [G, -]
last bean: G

Process finished with exit code 0
 */
// Update 2: 2 method tinGame (line 90) and takeOne (line 150) has been updated to PUBLIC .
// Update 3: Create 3 new public procedures (see details below): randInt, getBean, updateTin. In non-nerd langauges, this mean create a METHOD
// Update 4: (12.57 PM, March 14 '23): Add the BeansBag requirement using chatGPT, and temporarily add return 0 to int for the sake of running the code
// Update 5: (1.32 PM, March 14 '23)
/*
        - randInt is modified to completed in code
        - no import by scanner is allowed.
        - after running BeansBag, add a new line: BEANS_BAG: blue=10, green=10, available=10. This is consistent with PR2 requirements.

 */
// Update 6: takeOne modified
// Update 7: A lot of bugs. Haiyaa. Return to the original design/implementation for detail.
awss
```
## Grade update: 
- Holy shit this assignment got a 9.5 out of 10. I sometimes have no idea how the world works. 
