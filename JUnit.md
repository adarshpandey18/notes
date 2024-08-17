# JUnit ☕

JUnit is a framework for writing and running tests in Java.
It provides annotations and assertions that help developers verify that their code behaves as expected.

---

## Testing VS Unit Testing

- ### Testing
  - Broad term covering various methods to verify software functionality.
  - Includes unit tests, integration tests, system tests, acceptance tests, and more.
  - Ensure software works correctly in different scenarios and meets user requirements.  
  
- ### Unit Testing

  - Specific type of testing focused on individual units or components of software.
  - Tests the smallest testable parts like functions, methods, or classes in isolation.
  - Verify the correctness of each unit independently from the rest of the system.

---

## Steps

1. Prepare *(Setup test environment, write test methods)*
2. Provider test inputs.
3. Run the test.
4. Provide the expected output.
5. Report test result

> Every unit test have to be prepared differently for different units and need to provide different input & expected output.  

> In place of manually testing units we can use JUnit we have to provide input & expected output.

---

## Maven for JUnit

### 1. Create Project

### 2. Add Dependency [Maven Repository](https://mvnrepository.com/)

```<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.java-leanring</groupId>
    <artifactId>junit-learning</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>21</maven.compiler.source>
        <maven.compiler.target>21</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-api</artifactId>
            <version>5.11.0</version>
            <scope>test</scope>
        </dependency>

    </dependencies>

</project>

```
`<scope> test </scope>`  It is used for testing purpose not for deployment 

### 3. Create Test
**Sample Code :**
```
public class Calc {
  public int divide(int num1, int num2) {
      return num1 / num2;
    }
}
```

**Test Code :**
```
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;

public class CalcTest {
    @Test
    public void test() {
        Calc c = new Calc();
        int actualResult = c.divide(10, 5);
        int expectedResult = 2;
        assertEquals(expectedResult, actualResult);
    }
}
```
>Convention for naiming test  ClassNameTest for eg : CalcTest
### 4. Run Test


---
## @Test

- Applied over methods to mark methods as test.
- It is present inside `org.junit.junipter.api`
- Visibility of `@Test` annoted methods can be `public` `private` or `default`
- Also informs the engine what methods to run.
- The method only checks for failure if not then by default it is success.
> JUnit4 : The test method generated should be `default`.

> JUnit5 : `default` while generation it can be `public` `private` & `protected` 

---
## Assertions
Verification between **expected** and **actual** result.

<img src="https://github.com/adarshpandey18/notes/blob/main/images/assertions.png" alt="Description" width="300" height="200">


---
## Surefire Plugin
When projects goes into CI / CD their build process won't run the test cases so in order to run the whole project we have to integrate surefire.


---
## assertEquals()
It is used to check if two values are equal.
```
assertEquals("FirstValue", "SecondValue", "Message");
```

```
  @Test
  void testWithMessage() {
    int expected = 5;
    int actual = 4;
    assertEquals(expected, actual, "Expected value to be 5 but was 4");
  }
```
> Message is only displayed when the testcase is failing.
---
## assertArrayEquals()

### Only Successes when : 
  - Actual and expected arrays are equal.
  - Number of elements should match.
  - Elements of array are equal.
  - Order of elements in array


**TEST FAILS**
```
@Test
void testArray() {
  int[]expected = {1,2,3,4,5};
  int[]actual = {5,4,3,2,1};

  assertArrayEquals(expected, actual);
}
```

**TEST PASSED**
```
@Test
void testArray() {
  int[]expected = {1,2,3,4,5};
  int[]actual = {5,4,3,2,1};

  Arrays.sort(actual);
  
  assertArrayEquals(expected, actual);
}
```



