**LAB REPORT 5**

* Student's Post
  - Hello, I've been attempting to execute `test.sh` in order to test `ListExamples.java` file. However, upon doing so, I encountered an error. I suspect it might be related to the way the two sorted arrays are being combined within my `merge` method.
  ```
  [cs15lfa23rd@ieng6-203]:~:49$ bash test.sh
  JUnit version 4.13.2
  .E.E
  Time 0.012
  There were 2 failures:
  1) testMerge1(ListExamplesTests)
  java.lang.IndexOutOfBoundsException: Index: 2, Size: 2
        at java.util.ArrayList.rangeCheck(ArrayList.java:659)
        at java.util.ArrayList.get(ArrayList.java:435)
        at ListExamples.merge(ListExamples.java:28)
        at ListExamplesTests.testMerge1(ListExamplesTests.java:12)
  2) testMerge2 (ListExamplesTests)
  java.lang.IndexOutOfBoundsException: Index 3, Size: 3
        at java.util.ArrayList.rangeCheck(ArrayList.java:659)
        at java.util.ArrayList.get(ArrayList.java:435)
        at ListExamples.merge(ListExamples.java:28)
        at ListExamplesTests.testMerge1(ListExamplesTests.java:19)
        
  FAILURES!!!
  Tests run: 2, Failures: 2  
  ```

* TA's Post
  - It seems that the error is stemming from `ListExamples.java` file. Please use `vim` to look at that specific line or open it in an editor. Verify whether the indexes being used are within the bounds.

* Student's Response
  ```
  static List<String> merge(List<String> list1, List<String> list2) {
    List<String> result = new ArrayList<>();
    int index1 = 0, index2 = 0;
    while(index1 < list1.size() || index2 < list2.size()) {
      if(list1.get(index1).compareTo(list2.get(index2)) < 0) {
        result.add(list1.get(index1));
        index1 += 1;
      }
      else {
        result.add(list2.get(index2));
        index2 += 1;
      }
    }
    while(index1 < list1.size()) {
        "ListExamples.java" 50L, 1435C     
    }
  ```
- In the `ListExamples.java` file, the error seems to be triggered while evaluating the condition within an if statement.
- Upon inspection, I noticed that the while loop's condition utilizes the `||` logical operator.
- This signifies that the loop continues as long as at least one index remains in bounds.
- The issue lies here, causing the error. To resolve it, the logical operator should be changed to `&&`, ensuring the loop operates only when both indexes are within bounds.

* Information about the set up:
file Structure (from lab7):

```
lab7
  ListExamples.java  
  ListExamplesTests.java  
  test.sh  
```

ListExamples.java file includes:  

```
import java.util.ArrayList;
import java.util.List;

interface StringChecker { boolean checkString(String s); }

class ListExamples {

  // Returns a new list that has all the elements of the input list for which
  // the StringChecker returns true, and not the elements that return false, in
  // the same order they appeared in the input list;
  static List<String> filter(List<String> list, StringChecker sc) {
    List<String> result = new ArrayList<>();
    for(String s: list) {
      if(sc.checkString(s)) {
        result.add(0, s);
      }
    }
    return result;
  }

  // Takes two sorted list of strings (so "a" appears before "b" and so on),
  // and return a new list that has all the strings in both list in sorted order.
  static List<String> merge(List<String> list1, List<String> list2) {
    List<String> result = new ArrayList<>();
    int index1 = 0, index2 = 0;
    while(index1 < list1.size() && index2 < list2.size()) {
      if(list1.get(index1).compareTo(list2.get(index2)) < 0) {
        result.add(list1.get(index1));
        index1 += 1;
      }
      else {
        result.add(list2.get(index2));
        index2 += 1;
      }
    }
    while(index1 < list1.size()) {
      result.add(list1.get(index1));
      index1 += 1;
    }
    while(index2 < list2.size()) {
      result.add(list2.get(index2));
      // change index1 below to index2 to fix test
      index2 += 1;
    }
    return result;
  }
}
```

ListExamplesTests.java file includes:  

```
import java.util.ArrayList;
import java.util.List;

interface StringChecker { boolean checkString(String s); }

class ListExamples {

  // Returns a new list that has all the elements of the input list for which
  // the StringChecker returns true, and not the elements that return false, in
  // the same order they appeared in the input list;
  static List<String> filter(List<String> list, StringChecker sc) {
    List<String> result = new ArrayList<>();
    for(String s: list) {
      if(sc.checkString(s)) {
        result.add(0, s);
      }
    }
    return result;
  }

  // Takes two sorted list of strings (so "a" appears before "b" and so on),
  // and return a new list that has all the strings in both list in sorted order.
  static List<String> merge(List<String> list1, List<String> list2) {
    List<String> result = new ArrayList<>();
    int index1 = 0, index2 = 0;
    while(index1 < list1.size() || index2 < list2.size()) {
      if(list1.get(index1).compareTo(list2.get(index2)) < 0) {
        result.add(list1.get(index1));
        index1 += 1;
      }
      else {
        result.add(list2.get(index2));
        index2 += 1;
      }
    }
    while(index1 < list1.size()) {
      result.add(list1.get(index1));
      index1 += 1;
    }
    while(index2 < list2.size()) {
      result.add(list2.get(index2));
      // change index1 below to index2 to fix test
      index2 += 1;
    }
    return result;
  }
}
[jcuratolo@ieng6-201]:lab7:251$ cat ListExamplesTests.java
import static org.junit.Assert.*;
import org.junit.*;
import java.util.*;
import java.util.ArrayList;

public class ListExamplesTests {
        @Test(timeout = 500)
        public void testMerge1() {
                List<String> l1 = new ArrayList<String>(Arrays.asList("x", "y"));
                List<String> l2 = new ArrayList<String>(Arrays.asList("a", "b"));
                assertArrayEquals(new String[]{ "a", "b", "x", "y"}, ListExamples.merge(l1, l2).toArray());
        }

        @Test(timeout = 500)
        public void testMerge2() {
                List<String> l1 = new ArrayList<String>(Arrays.asList("a", "b", "c"));    
                List<String> l2 = new ArrayList<String>(Arrays.asList("c", "d", "e"));    
                assertArrayEquals(new String[]{ "a", "b", "c", "c", "d", "e" }, ListExamples.merge(l1, l2).toArray());
        }
}
```

test.sh file includes: 

```
javac -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar *.java
java -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore ListExamplesTests
```

Command line to trigger bug:  

`bash test.sh`  

Description of what to edit to fix the bug:   

- Change line 27 in  `ListExamples.java`
- Update the `while` loop condition from `index1 < list1.size() || index2 < list2.size()` to `index1 < list1.size() && index2 < list2.size()`

**PART 2**  

Recently, I learned how to use a tool called `vim` for editing files on a computer that doesn't have normal buttons and windows. At first, `vim` was a bit hard to understand and use because it's different from what I'm used to. But with practice, I got better at it. Even though I still don't love using it, I can see how helpful it is when I can only use a simple screen.

I also figured out how to connect to `GitHub`, a place where people share code, using a special way called `SSH` keys on the same computer. This let me upload my changes without needing a special program like `GitHub Desktop`. Learning these things helped me work better when I could use only use a basic screen and commands.