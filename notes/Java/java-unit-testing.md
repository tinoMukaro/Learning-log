# Java Unit Testing

These notes cover basic Java unit testing with **JUnit 5** only.

No Mockito, Spring context, database, or integration testing yet.

---

## 1. What is a unit test?

A unit test checks one small piece of code, usually one method, in isolation.

Example:

```java
BigDecimal result = calculator.add(
    new BigDecimal("10.00"),
    new BigDecimal("5.00")
);

assertEquals(new BigDecimal("15.00"), result);
```

The basic pattern is:

```text
Arrange → Act → Assert
```

- **Arrange:** Prepare the input and objects.
- **Act:** Call the method being tested.
- **Assert:** Check that the result is correct.

---

## 2. Basic test class

```java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.*;

class CalculatorTest {

    @Test
    void shouldAddTwoNumbers() {
        int result = 2 + 3;

        assertEquals(5, result);
    }
}
```

Important points:

- The test class usually ends with `Test`.
- A test method uses `@Test`.
- Test methods normally return `void`.
- Test method names should explain the scenario and expected behaviour.

---

## 3. `@Test`

`@Test` tells JUnit that a method is a test case.

```java
@Test
void shouldCalculateEmployeeContribution() {
    // test code
}
```

Without `@Test`, JUnit will not run the method as a test.

---

## 4. `@BeforeEach`

`@BeforeEach` runs before every test method.

Use it for setup shared by multiple tests.

```java
class NssaCalculatorTest {

    private NssaCalculator calculator;

    @BeforeEach
    void setUp() {
        calculator = new NssaCalculator();
    }

    @Test
    void shouldCalculateContribution() {
        // calculator is ready
    }

    @Test
    void shouldApplyContributionCeiling() {
        // a fresh calculator is ready again
    }
}
```

Execution order:

```text
setUp()
firstTest()

setUp()
secondTest()
```

This helps prevent test data or object state from accumulating between tests.

Only place genuinely shared setup in `@BeforeEach`. Scenario-specific data should normally stay inside its test.

---

## 5. `@AfterEach`

`@AfterEach` runs after every test.

```java
@AfterEach
void cleanUp() {
    // cleanup code
}
```

It is useful when a test opens files, connections, or other resources that must be closed.

For simple calculation tests, you usually do not need it.

---

## 6. `@BeforeAll` and `@AfterAll`

`@BeforeAll` runs once before all tests in the class.

`@AfterAll` runs once after all tests in the class.

```java
@BeforeAll
static void startTests() {
    System.out.println("Starting test class");
}

@AfterAll
static void finishTests() {
    System.out.println("Finished test class");
}
```

They are usually `static`.

Use them for expensive setup shared by the whole test class, not for resetting data before each test.

---

## 7. Arrange, Act, Assert

A clean test can be divided into three sections.

```java
@Test
void calculateDifferentEmployeeAndEmployerRates() {
    // Arrange
    NssaParameters differentRates = new NssaParameters(
        new BigDecimal("0.045"),
        new BigDecimal("0.020"),
        new BigDecimal("1000.00"),
        "DIFFERENT-RATES"
    );

    // Act
    NssaCalculation result = calculator.calculate(
        new BigDecimal("500.00"),
        differentRates
    );

    // Assert
    assertEquals(
        new BigDecimal("22.50"),
        result.employeeContribution()
    );

    assertEquals(
        new BigDecimal("10.00"),
        result.employerContribution()
    );
}
```

You do not have to write the comments every time, but the structure should be clear.

---

## 8. Common assertions

Assertions check whether the actual result matches the expected result.

Import them with:

```java
import static org.junit.jupiter.api.Assertions.*;
```

### `assertEquals`

Checks that two values are equal.

```java
assertEquals(10, result);
```

Order:

```java
assertEquals(expected, actual);
```

Example:

```java
assertEquals(
    new BigDecimal("22.50"),
    result.employeeContribution()
);
```

---

### `assertNotEquals`

Checks that two values are not equal.

```java
assertNotEquals(0, result);
```

---

### `assertTrue`

Checks that a condition is true.

```java
assertTrue(employee.isActive());
```

---

### `assertFalse`

Checks that a condition is false.

```java
assertFalse(employee.isSuspended());
```

---

### `assertNull`

Checks that a value is `null`.

```java
assertNull(employee.getTerminationDate());
```

---

### `assertNotNull`

Checks that a value is not `null`.

```java
assertNotNull(result);
```

---

### `assertSame`

Checks that two variables point to the exact same object.

```java
assertSame(expectedObject, actualObject);
```

This checks object identity, not just equal values.

---

### `assertAll`

Groups several assertions so JUnit checks all of them.

```java
assertAll(
    () -> assertEquals(
        new BigDecimal("22.50"),
        result.employeeContribution()
    ),
    () -> assertEquals(
        new BigDecimal("10.00"),
        result.employerContribution()
    )
);
```

Without `assertAll`, JUnit stops that test at the first failed assertion.

---

## 9. Testing exceptions with `assertThrows`

Use `assertThrows` when the expected result is an exception.

```java
@Test
void shouldRejectNegativeSalary() {
    NssaParameters parameters = new NssaParameters(
        new BigDecimal("0.045"),
        new BigDecimal("0.045"),
        new BigDecimal("1000.00"),
        "STANDARD"
    );

    IllegalArgumentException exception = assertThrows(
        IllegalArgumentException.class,
        () -> calculator.calculate(
            new BigDecimal("-500.00"),
            parameters
        )
    );

    assertEquals(
        "Salary cannot be negative",
        exception.getMessage()
    );
}
```

The lambda:

```java
() -> calculator.calculate(...)
```

means:

> Run this code and expect it to throw the specified exception.

---

## 10. Testing that no exception is thrown

```java
assertDoesNotThrow(
    () -> calculator.calculate(
        new BigDecimal("500.00"),
        parameters
    )
);
```

Use this when successful execution itself is what you want to verify.

---

## 11. BigDecimal in tests

For money, create `BigDecimal` values from strings:

```java
new BigDecimal("500.00")
```

Avoid:

```java
new BigDecimal(500.00)
```

Using a `double` may introduce floating-point precision problems.

### Scale matters with `assertEquals`

These values are numerically equal:

```java
new BigDecimal("10.0")
new BigDecimal("10.00")
```

But `BigDecimal.equals()` considers both value and scale, so:

```java
assertEquals(
    new BigDecimal("10.0"),
    new BigDecimal("10.00")
);
```

can fail.

For money calculations, make the application return a consistent scale such as two decimal places:

```java
amount.setScale(2, RoundingMode.HALF_UP);
```

Then use matching expected values:

```java
new BigDecimal("10.00")
```

Another numerical comparison option is:

```java
assertEquals(
    0,
    expected.compareTo(actual)
);
```

`compareTo()` ignores scale differences.

---

## 12. Good test method names

A test name should describe the behaviour being tested.

Common styles:

```java
void shouldCalculateEmployeeContribution()
```

```java
void shouldUseDifferentEmployeeAndEmployerRates()
```

```java
void shouldApplyMaximumContributionCeiling()
```

```java
void shouldRejectNegativeSalary()
```

```java
void returnsZeroContributionWhenSalaryIsZero()
```

Avoid vague names:

```java
void test1()
```

```java
void calculatorTest()
```

A good test name helps explain the business rule when a test fails.

---

## 13. One scenario per test

Each test should focus on one scenario or business rule.

Good:

```java
@Test
void shouldCalculateDifferentEmployeeAndEmployerRates() {
}
```

```java
@Test
void shouldApplyContributionCeiling() {
}
```

```java
@Test
void shouldRejectNegativeSalary() {
}
```

Avoid putting many unrelated scenarios into one huge test.

Small tests make failures easier to understand.

---

## 14. Tests should be independent

A test should not depend on another test running first.

Bad idea:

```java
@Test
void createEmployee() {
    employee = new Employee("Tino");
}

@Test
void calculateSalary() {
    // depends on createEmployee() running first
}
```

JUnit does not guarantee that tests run in the order you wrote them.

Each test should prepare its own required state.

---

## 15. Do not test implementation details

Test what the method produces or how it behaves, not every internal line.

Good:

```java
assertEquals(
    new BigDecimal("22.50"),
    result.employeeContribution()
);
```

Usually unnecessary:

- Checking private variables directly
- Testing private methods separately
- Copying the production calculation into the test
- Asserting internal steps that users of the method cannot observe

The test should verify the public behaviour.

---

## 16. Reusable helper methods

If many tests need similar objects, create a helper method.

```java
private NssaParameters standardParameters() {
    return new NssaParameters(
        new BigDecimal("0.045"),
        new BigDecimal("0.045"),
        new BigDecimal("1000.00"),
        "STANDARD"
    );
}
```

Then:

```java
@Test
void shouldCalculateStandardContribution() {
    NssaCalculation result = calculator.calculate(
        new BigDecimal("500.00"),
        standardParameters()
    );

    assertEquals(
        new BigDecimal("22.50"),
        result.employeeContribution()
    );
}
```

A helper method is useful when it improves readability.

Do not hide the important values for a special scenario. Keep unusual values directly inside that test.

---

## 17. `@DisplayName`

`@DisplayName` gives a test a readable label in test reports.

```java
@Test
@DisplayName("Calculates different employee and employer contribution rates")
void calculateDifferentEmployeeAndEmployerRates() {
}
```

It is optional. A clear method name is often enough.

---

## 18. `@Disabled`

`@Disabled` temporarily prevents a test from running.

```java
@Test
@Disabled("Waiting for contribution ceiling rule")
void shouldApplyContributionCeiling() {
}
```

Do not leave tests disabled without a clear reason.

---

## 19. Parameterized tests

A parameterized test runs the same test logic using different inputs.

```java
@ParameterizedTest
@CsvSource({
    "100.00, 4.50",
    "500.00, 22.50",
    "1000.00, 45.00"
})
void shouldCalculateEmployeeContribution(
        String salary,
        String expectedContribution
) {
    NssaCalculation result = calculator.calculate(
        new BigDecimal(salary),
        standardParameters()
    );

    assertEquals(
        new BigDecimal(expectedContribution),
        result.employeeContribution()
    );
}
```

Required imports:

```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvSource;
```

Use this when the same rule needs to be checked with several values.

For now, normal `@Test` methods are completely fine while learning.

---

## 20. Common test scenarios

When Codex gives you a method to test, consider these categories.

### Normal case

Does it produce the correct result for ordinary valid input?

```java
salary = 500.00
```

### Boundary case

What happens exactly at a limit?

```java
salary = contributionCeiling
```

### Below the boundary

```java
salary = contributionCeiling - 0.01
```

### Above the boundary

```java
salary = contributionCeiling + 0.01
```

### Zero

```java
salary = 0.00
```

### Negative value

```java
salary = -1.00
```

### Null

```java
parameters = null
```

Only test `null` if the method is expected to handle or reject it.

### Different configurations

```java
employeeRate = 0.045
employerRate = 0.020
```

### Rounding

Test values that produce more than two decimal places.

```java
salary = 333.33
rate = 0.045
```

---

## 21. Example NSSA test class

```java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import java.math.BigDecimal;

import static org.junit.jupiter.api.Assertions.*;

class NssaCalculatorTest {

    private NssaCalculator calculator;

    @BeforeEach
    void setUp() {
        calculator = new NssaCalculator();
    }

    @Test
    void shouldCalculateDifferentEmployeeAndEmployerRates() {
        // Arrange
        NssaParameters differentRates = new NssaParameters(
            new BigDecimal("0.045"),
            new BigDecimal("0.020"),
            new BigDecimal("1000.00"),
            "DIFFERENT-RATES"
        );

        // Act
        NssaCalculation result = calculator.calculate(
            new BigDecimal("500.00"),
            differentRates
        );

        // Assert
        assertAll(
            () -> assertEquals(
                new BigDecimal("22.50"),
                result.employeeContribution()
            ),
            () -> assertEquals(
                new BigDecimal("10.00"),
                result.employerContribution()
            )
        );
    }

    @Test
    void shouldReturnZeroContributionsForZeroSalary() {
        NssaParameters parameters = standardParameters();

        NssaCalculation result = calculator.calculate(
            new BigDecimal("0.00"),
            parameters
        );

        assertAll(
            () -> assertEquals(
                new BigDecimal("0.00"),
                result.employeeContribution()
            ),
            () -> assertEquals(
                new BigDecimal("0.00"),
                result.employerContribution()
            )
        );
    }

    @Test
    void shouldRejectNegativeSalary() {
        NssaParameters parameters = standardParameters();

        assertThrows(
            IllegalArgumentException.class,
            () -> calculator.calculate(
                new BigDecimal("-1.00"),
                parameters
            )
        );
    }

    private NssaParameters standardParameters() {
        return new NssaParameters(
            new BigDecimal("0.045"),
            new BigDecimal("0.045"),
            new BigDecimal("1000.00"),
            "STANDARD"
        );
    }
}
```

---

## 22. Quick test-writing template

```java
@Test
void shouldDescribeExpectedBehaviour() {
    // Arrange
    Input input = ...;
    ExpectedConfiguration configuration = ...;

    // Act
    Result result = objectUnderTest.method(input, configuration);

    // Assert
    assertEquals(expectedValue, result.value());
}
```

Exception template:

```java
@Test
void shouldRejectInvalidInput() {
    // Arrange
    Input invalidInput = ...;

    // Act and Assert
    ExceptionType exception = assertThrows(
        ExceptionType.class,
        () -> objectUnderTest.method(invalidInput)
    );

    assertEquals("Expected message", exception.getMessage());
}
```

---

## 23. Quick reference

```text
@Test
Marks a method as a test.

@BeforeEach
Runs before every test. Used for fresh shared setup.

@AfterEach
Runs after every test. Used for cleanup.

@BeforeAll
Runs once before all tests in the class.

@AfterAll
Runs once after all tests in the class.

assertEquals(expected, actual)
Checks equality.

assertTrue(condition)
Checks that a condition is true.

assertFalse(condition)
Checks that a condition is false.

assertNull(value)
Checks that a value is null.

assertNotNull(value)
Checks that a value is not null.

assertThrows(Exception.class, lambda)
Checks that code throws an expected exception.

assertDoesNotThrow(lambda)
Checks that code completes without throwing.

assertAll(...)
Runs several assertions and reports all failures.

@DisplayName
Adds a readable test name.

@Disabled
Temporarily skips a test.
```

---

## 24. Final checklist

Before finishing a test, ask:

- Does the name clearly describe the scenario?
- Did I arrange all required input?
- Did I call the real method being tested?
- Did I assert the expected result?
- Is the expected value written before the actual value?
- Is this test independent from other tests?
- Am I testing one main behaviour?
- Did I include relevant edge or boundary cases?
- For money, did I use `BigDecimal` strings?
- Would the failure message make sense to me later?

---

## Main idea

A good unit test says:

> Given this input, when I call this method, then I expect this result or exception.

```text
Given → When → Then
Arrange → Act → Assert
```
