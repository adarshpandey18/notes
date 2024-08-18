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
<img src="https://github.com/adarshpandey18/notes/blob/main/images/create_new_project.png" width=400 height=400>

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
<img src="https://github.com/adarshpandey18/notes/blob/main/images/test_success.png" width=250 height=200>

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

<img src="https://github.com/adarshpandey18/notes/blob/main/images/assertions.png">


---
## Surefire Plugin
When projects goes into CI / CD their build process won't run the test cases so in order to run the whole project we have to integrate surefire.
```
<build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <version>3.0.0-M5</version>
    </plugin>
  </plugins>
</build>
```


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
---
## Testing Performance

**Sample Code :**
```
public class SortingArray {    
  public int[] sortingArray(int[]array) {
    for(int i = 0; i < 100000000; i++) {
      Arrays.sort(array);
    }
    return array;
  }
}
```

**Test Code :**
```
public class TestingPerformance {
  @Test
  void sortingArrayTest() {
    SortingArray sa = new SortingArray();
    int[]unsorted = {5,4,3,2,1};
    assertTimeout(Duration.ofMillis(10), ()-> sa.sortingArray(unosrted));
  }
}
```
**Test Case Failed :**
```
org.opentest4j.AssertionFailedError: execution exceeded timeout of 10 ms by 643 ms
  ```
> For JUnit4 `@Test(timeout=10)` is used.

---
## BeforeEach & AfterEach
### @BeforeEach
  Before each Test Case the method will be executed.
### @AfterEach 
  After each Test Case the method will be executed.

**Example :**
```
public class Annotations {
  @BeforeEach
  void init() {
    System.out.println("Before Each Executed");
  }

  @Test
  void sampleTest1() {
    System.out.println("Test 1");
  }

  @Test
  void sampleTest2() {
    System.out.println("Test 2");
  }
    
  @AfterEach
  void dispose() {
    System.out.println("After Each Statement Executed");
  }
}
```
**Output :**
```
Before Each Executed
Test 1
After Each Statement Executed
Before Each Executed
Test 2
After Each Statement Executed
```
---
## BeforeAll & AfterAll
### @BeforeAll
  It executes only once before all Test Cases executed.
### @AfterAll 
  It executes only once after all Test Cases are executed.

**Example :**
```
public class Annotations {
  @BeforeAll
  static void beforeAll(){
      System.out.println("Before All Executed");
  }

  @Test
  void sampleTest1() {
      System.out.println("Test 1");
  }

  @Test
  void sampleTest2() {
      System.out.println("Test 2");
   }

  @AfterAll
  static void afterAll(){
      System.out.println("After All Executed");
  }
}
```
**Output :**
```
Before All Executed
Test 1
Test 2
After All Executed
```
---
## TestInstance Behavior
In JUnit 5, a new instance of the test class is created for each test method by default. This means that if you have multiple test methods in a test class, JUnit will instantiate the test class once for each method before running that method for that reason the `@BeforeAll` and `@AfterAll`  methods are declared static and they are called once before the instance of test class is created.  

In order to change the behaveious or TestInstance we use `@TestInstance()` annotation.

- `@TestInstance(TestInstance.Lifecycle.PER_METHOD)` : It is default behavior. For every test method the instance is getting created.

- `@TestInstance(TestInstance.Lifecycle.PER_CLASS)` : Multiple instance are not created. ***No need to use static methods for `@BeforeAll` & `@AfterAll` as now instancee is not getting created for every test method.***

```
@TestInstance(TestInstance.Lifecycle.PER_CLASS)
public class Annotations {
  @BeforeAll
  void beforeAll(){
      System.out.println("Before All Executed");
  }

  @Test
  void sampleTest1() {
      System.out.println("Test 1");
  }

  @Test
  void sampleTest2() {
      System.out.println("Test 2");
  }

  @AfterAll
  void afterAll() {
      System.out.println("After All Executed");
  }
}
```