# Iterating Over The Characters In A String

## Learning Goals

- Write a loop to access each character in a string
- Determine the presence or quantity of specific characters in a string
- Test for characters that fall within a range of ASCII decimal values

## Introduction

We often need to iterate through the individual characters in a string
to look for certain characters. For example, a password checker
might want to confirm that a string
contains at least one uppercase letter and at least one digit.

In this lesson, we will look at a few techniques to iterate 
through a string and gather data about the individual characters.

## Counting the occurrences of a specific character in a string

Suppose we would like to count the number of occurrences of the character 'A'
contained within a string.  We will use a loop to get each character for comparison.

Recall from the previous lesson we can convert a `String` into an array of `char`:

```java
String str = "hello";
char[] arrayOfChars = str.toCharArray();
```
 
We can use an index-based loop to access and print each individual character in the array:

```java
String str = "hello";
char[] arrayOfChars = str.toCharArray();
for (int i=0;i<arrayOfChars.length; i++) {
    System.out.println(arrayOfChars[i]);
}
```

The code prints:

```text
h
e
l
l
o
```

If we don't need to know the position/index of each character,
we can also use an enhanced for loop like the example below.
Each iteration of the loop assigns the variable `c` to the next character in the array:

```java
String str = "hello";
char[] arrayOfChars = str.toCharArray();
for (char c : arrayOfChars) {
    System.out.println(c);
}
```

Since the only thing we do with `arrayOfChars` is iterate through it
in the `for` loop, we can rewrite this as:

```java
String str = "hello";
for (char c : str.toCharArray()) {
    System.out.println(c);
}
```

We get the same output:

```text
h
e
l
l
o
```

Let's create a class `StringSearchUtility`
that provides several useful methods for searching
the contents of a string.  We'll add a method named `countA`
to count how many times the letter 'A' appears in a string.
The method iterates over the characters in the parameter
string, incrementing a counter if the character is an 'A':

```java
public static int countA(String str) {
    int count = 0;
    for (char c : str.toCharArray()) {
        if (c == 'A') {
            count++;
        }
    }
    return count;
}
```


Let's add a `main` method to print the result of calling `countA`
with a few different strings.  Note we are specifically looking
for uppercase 'A', thus the string "amazing" should return a count of 0.

```java
public class StringSearchUtility {

    public static int countA(String str) {
        int count = 0;
        for (char c : str.toCharArray()) {
            if (c == 'A') {
                count++;
            }
        }
        return count;
    }

    public static void main(String[] args) {
        System.out.println(countA("BABY")); //1
        System.out.println(countA("BANANA")); //3
        System.out.println(countA("123!")); //0
        System.out.println(countA("")); //0
        System.out.println(countA("amazing")); //0
    }

}
```

The program should print:

```text
1
3
0
0
0
```

Rather than relying on visual inspection of the output produced by the `main` method,
we should create a Junit class `StringSearchUtilityTest` to test the value
returned by the `StringSearchUtility.countA` method (we preface the class name since it is
a static method in another class).

```java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.*;

public class StringSearchUtilityTest {

    @Test
    public void countA() {
        assertEquals(1, StringSearchUtility.countA("BABY"));
        assertEquals(3, StringSearchUtility.countA("BANANA"));
        assertEquals(0, StringSearchUtility.countA("123!"));
        assertEquals(0, StringSearchUtility.countA(""));
        assertEquals(0, StringSearchUtility.countA("amazing"));
    }

}
```

Running the Junit test class `StringSearchUtilityTest` should result in the test passing.


## Testing for the presence of a character in a string

Let's add a predicate method `containsA` that returns a `boolean` value indicating
whether a string contains the character 'A'.  In a subsequent lesson we'll see there
are existing `String` methods that we could use, but for now we'll implement this
manually to show an example method that returns a value from within a loop, as well
as a different value after the loop.

One approach to implement the `containsA` method is to simply have it
call the `countA` method and test if the result is positive.  However,
what if some of our strings contain lots of characters?  Rather than iterating through
the long sequence of characters to count the total number of 'A' characters,
it would be more efficient to return `true` as soon as we find the first 'A'.

We'll add our new method `containsA` to the `StringSearchUtility` class.
Note that the method returns `true` in the for loop as soon as it finds an 'A',
but only returns `false` after looping through the entire
string and not finding any 'A' characters.


```java
public static boolean containsA(String str) {
    for (char c : str.toCharArray()) {
        if (c == 'A') {
            return true;
        }
    }
    return false;
}
```

NOTE: A common error is to use an `if-else` as shown below.  But this code will only
loop once, returning a boolean value based on whether the
first character in the string is an 'A', it never looks
at the rest of the string. You can use the Java Visualizer to step through
[this example](https://pythontutor.com/visualize.html#code=public%20class%20IterationError%20%7B%0A%20%20%20%20public%20static%20boolean%20containsA%28String%20str%29%20%7B%0A%20%20%20%20%20%20%20%20for%20%28char%20c%20%3A%20str.toCharArray%28%29%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20if%20%28c%20%3D%3D%20'A'%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20return%20true%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20else%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20return%20false%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20return%20false%3B%0A%20%20%20%20%7D%0A%0A%20%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20%20%20%20%20//should%20print%20true%20since%20string%20contains%20'A',%20but%20prints%20false%0A%20%20%20%20%20%20%20%20System.out.println%28containsA%28%22GOOD%20DAY!%22%29%29%3B%0A%20%20%20%20%7D%0A%0A%7D%0A&cumulative=false&curInstr=0&heapPrimitives=true&mode=display&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false) and see the error.
The correct code shown above waits until *after* the loop has finished
executing to return `false`.

<table>
<tr>
<th>Don't write this!</th>
</tr>
<tr>
<td>

<pre>
<code>

public static boolean containsA(String str) {
    for (char c : str.toCharArray()) {
        if (c == 'A') {
            return true;
        }
        else {
            return false;
        }
    }
    return false;
}

</code>
</pre>

</td></tr>
</table>




Let's update the Junit class `StringSearchUtilityTest` with a new test method:

```java
@Test
public void containsA() {
    assertTrue(StringSearchUtility.containsA("BABY"));
    assertTrue(StringSearchUtility.containsA("BANANA"));
    assertFalse(StringSearchUtility.containsA("123!"));
    assertFalse(StringSearchUtility.containsA(""));
    assertFalse(StringSearchUtility.containsA("amazing"));
}
```

Running the Junit test class `StringSearchUtilityTest` should result in both tests passing.


## Testing for characters within an ASCII range 

Let's add another utility method named `containsUppercase`
to check whether a string contains an uppercase letter 'A' through 'Z'.

We could simply use the logical operator `||` (or) to compare each character in the string
with each uppercase letter, but that would be an annoying boolean
expression to have to write:

```java
public static boolean containsUppercase(String str) {
    for (char c : str.toCharArray()) {
        if (c == 'A' || c == 'B' || c == 'C' || ....... || c == 'Z' ) {
            return true;
        }
    }
    return false;
}
```

Notice the decimal values for the uppercase letters in the ASCII table:

![ASCII Table](https://curriculum-content.s3.amazonaws.com/6676/java-mod2-strings/ascii_uppercase.png)

Recall from the previous lesson that we can treat a `char` as a decimal value,
so 'A' and 65 represent the same character, and 'Z' and 90 represent the same character.
Thus, we can check if a character is an uppercase letter by testing if
it falls within an inclusive range of decimal values `65` and `90`,
or we can simply use 'A' and 'Z' for the range:


```java
public static boolean containsUppercase(String str) {
    for (char c : str.toCharArray()) {
        if (c >= 'A' && c <= 'Z') {
            return true;
        }
    }
    return false;
}
```

Let's add a Junit test method:

```java
@Test
public void containsUppercase() {
    assertTrue(StringSearchUtility.containsUppercase("BABY"));
    assertTrue(StringSearchUtility.containsUppercase("baBy"));
    assertTrue(StringSearchUtility.containsUppercase("Yes"));
    assertFalse(StringSearchUtility.containsUppercase("hello!"));
    assertFalse(StringSearchUtility.containsUppercase(""));
}
```

Running the Junit test class `StringSearchUtilityTest` should result in all test methods passing.


## Comprehension Check

<details>
   <summary>Look at the ASCII table. What range of characters would the method count?</summary>

  <p>83 and 90 represent the range 'S' to 'Z'</p>

</details>


```java
public static int countCharsInRange(String str) {
    int count = 0;
    for (char c : str.toCharArray()) {
        if (c >= 83 && c <= 90 ) {
            count++;
        }
    }
    return count;
}
```


<details>
   <summary>Consider the code shown below.
   The code should print <code>true</code> since the string contains the '<' character,
   but it prints <code>false</code>.  Can you explain the error?  Perhaps use IntelliJ's debugger or
   the <a href="https://pythontutor.com/java.html#mode=edit">Java Visualizer</a> and step through
   the loop to identify the error.</summary>

  <p>The for loop should not contain an <code>else</code>.  The value <code>false</code>
  should only be returned after the for loop has finished executing, allowing the entire string to be searched.
  The <code>else</code> causes the method to only look at the first character in the string.</p>

</details>

```java
public class Example2 {
    public static boolean containsComparisonOperator(String str) {
        for (char c : str.toCharArray()) {
            if (c == '<' || c == '>') {
                return true;
            }
            else {
                return false;
            }
        }
        return false;
    }

    public static void main(String[] args) {
        //should print true since string contains '<', but prints false
        System.out.println(containsComparisonOperator("123 < 100"));
    }

}
```



## Code Check

The  `StringSearchUtility` class:

```java
public class StringSearchUtility {

    public static int countA(String str) {
        int count = 0;
        for (char c : str.toCharArray()) {
            if (c == 'A') {
                count++;
            }
        }
        return count;
    }

    public static boolean containsA(String str) {
        for (char c : str.toCharArray()) {
            if (c == 'A') {
                return true;
            }
        }
        return false;
    }

    public static boolean containsUppercase(String str) {
        for (char c : str.toCharArray()) {
            if (c >= 'A' && c <= 'Z') {
                return true;
            }
        }
        return false;
    }

}
```

The `StringSearchUtilityTest` class:

```java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.*;

public class StringSearchUtilityTest {

    @Test
    public void countA() {
        assertEquals(1, StringSearchUtility.countA("BABY"));
        assertEquals(3, StringSearchUtility.countA("BANANA"));
        assertEquals(0, StringSearchUtility.countA("123!"));
        assertEquals(0, StringSearchUtility.countA(""));
        assertEquals(0, StringSearchUtility.countA("amazing"));
    }

    @Test
    public void containsA() {
        assertTrue(StringSearchUtility.containsA("BABY"));
        assertTrue(StringSearchUtility.containsA("BANANA"));
        assertFalse(StringSearchUtility.containsA("123!"));
        assertFalse(StringSearchUtility.containsA(""));
        assertFalse(StringSearchUtility.containsA("amazing"));
    }

    @Test
    public void containsUppercase() {
        assertTrue(StringSearchUtility.containsUppercase("BABY"));
        assertTrue(StringSearchUtility.containsUppercase("baBy"));
        assertTrue(StringSearchUtility.containsUppercase("Yes"));
        assertFalse(StringSearchUtility.containsUppercase("hello!"));
        assertFalse(StringSearchUtility.containsUppercase(""));
    }

}
```

## Conclusion

A `String` stores a sequence of characters.  We can convert
a `String` to an array of `char`, then loop through
the array to access each individual character.  Due to their
decimal representation, we can easily test
if a character falls within a range of values.

