# Unit Testing
Unit testing is a software testing technique where individual units or components of a software application are tested in isolation. The goal is to validate that each unit of the software performs as expected. Unit tests are typically automated and written by developers as they work on the codebase.

## Why Unit Testing?
Unit testing is important for several reasons:
1. **Early Bug Detection**: Unit tests help identify bugs and issues early in the development process, making it easier to fix them before they become more complex and costly to resolve.
2. **Code Quality**: Writing unit tests encourages developers to write cleaner, more modular code. It promotes better design practices and helps ensure that each unit of code has a single responsibility.
3. **Documentation**: Unit tests serve as documentation for the codebase. They provide examples of how to use the code and what its expected behavior is.
4. **Refactoring Confidence**: When making changes or refactoring code, having a comprehensive suite of unit tests allows developers to ensure that existing functionality is not broken.
5. **Regression Prevention**: Unit tests help prevent regressions by ensuring that previously working code continues to function as expected after changes are made.
6. **Faster Development**: Although writing unit tests may seem time-consuming at first, it can lead to faster development in the long run by reducing the time spent on debugging and fixing issues.
7. **Continuous Integration**: Unit tests can be integrated into continuous integration (CI) pipelines, allowing for automated testing of code changes before they are merged into the main codebase.

---

## Test Driven Development (TDD)
Test Driven Development (TDD) is a software development approach where tests are written before the actual code.
The process typically follows these steps:
1. **Write a Test**: Write a test for a specific functionality or feature that you want to implement.
2. **Run the Test**: Run the test to see it fail. This confirms that the test is valid and that the functionality is not yet implemented.
3. **Write the Code**: Write the minimum amount of code necessary to make the test pass.
4. **Run the Test Again**: Run the test again to see it pass. This confirms that the code works as expected.
5. **Refactor**: Refactor the code to improve its design and maintainability while ensuring that the test still passes.
6. **Repeat**: Repeat the process for the next piece of functionality.
TDD encourages developers to think about the design and requirements of the code before writing it, leading to better software architecture and fewer bugs.

---

## Unit Testing Frameworks
There are several popular unit testing frameworks available for different programming languages. Here are a few examples:
- **Python**: `unittest`, `pytest`
- **Java**: `JUnit`
- **JavaScript**: `Jest`, `Mocha`
- **PHP**: `PHPUnit`
- **Go**: `testing` package
- **Swift**: `XCTest`
- **Rust**: `cargo test`
- **Dart**: `test` package
- **C++**: `Google Test`, `Catch2`
- **Objective-C**: `XCTest`

--- 

## Best Practices for Unit Testing
1. **Keep Tests Isolated**: Each unit test should test a single unit of code in isolation. Avoid dependencies on external systems or shared state.
2. **Use Descriptive Names**: Use descriptive names for test cases to clearly indicate what functionality is being tested.
3. **Test for Edge Cases**: Include tests for edge cases and unexpected inputs to ensure robustness.
4. **Use Mocks and Stubs**: Use mocks and stubs to simulate dependencies and isolate the unit being tested.

---

## Jest & Vitest
Jest and Vitest are popular JavaScript testing frameworks that provide a rich set of features for unit testing. They are widely used in the JavaScript ecosystem, especially for testing React applications.

### Jest
Jest is a JavaScript testing framework developed by Facebook. It is known for its simplicity, ease of use, and powerful features. Some key features of Jest include:
- **Zero Configuration**: Jest works out of the box with minimal configuration, making it easy to get started.
- **Snapshot Testing**: Jest allows you to take snapshots of your components and compare them to previous versions, making it easy to catch unexpected changes.
- **Mocking**: Jest provides built-in mocking capabilities, allowing you to mock functions, modules, and timers.
- **Code Coverage**: Jest can generate code coverage reports, helping you identify untested parts of your codebase.

### Vitest
Vitest is a modern testing framework that is designed to be fast and efficient. It is built on top of Vite, a fast build tool for modern web applications.

---
 
## Install

```bash
npm i -D jest vitest
```

---

## Usage

**Testing Success**
```javascript
import { describe, it, expect } from 'vitest'
import { add } from "./math";

describe('add', () => {
    it('should add all number in array', () => {
        const result = add([1, 2, 3, 4, 5]);
        expect(result).toBe(15);
    })
    it('should return NaN if array is having non numeric value', () => {
        const result = add([1,2, "sd"]);
        expect(result).toBeNaN();
    })
})
```

**Testing for Error**
```javascript
import { describe, it, expect } from 'vitest'
import { add } from "./math";

describe('add', () => {
    it('should not throw error if no value is passed into function', () => {
        const resultFn = () => add();
        expect(resultFn).not.toThrow();
    })

    it('should throw error if multiple argument is passed insted of array', () => {
        const resultFn = () => add(1, 2, 3, 4, 5);
        expect(resultFn).toThrow();
    })

    it('should throw error if multiple argument is passed insted of array', () => {
        const resultFn = () => add(1, 2, 3, 4, 5);
        expect(resultFn).toThrow(/number is not iterable/);
    })
})
```

**Testing Async Function**
```javascript
import { describe, it, expect } from 'vitest'
import { add } from "./math";

describe('add', () => {
    it('should add all number in array', async (done) => {
        add([1, 2, 3, 4, 5], (result, error) => {
            try{
                expect(result).toBe(15);
                done();
            }
            catch(error){
                done(error);
            }
        })
    })
})

describe('add', () => {
    expect(add([1, 2, 3, 4, 5])).resolves.toBe(15);
    expect(add([1, 2, 3, 4, "dd"])).rejects.toBeNaN();
})
```

---

## Spies
Spies are a type of mock that allows you to track how often a function is called and with what arguments. They are useful for testing side effects, such as updating state or making API calls.

```javascript
import { describe, it, expect, vi } from 'vitest'
import { writeFile } from "./writeFile";

describe('write File', () => {
    it("Should write file with test words in text.txt", () => {
        const spy = vi.spyOn(fs, "writeFile");
        writeFile("text.txt", "test words");
        expect(spy).toHaveBeenCalledWith("text.txt", "test words");
    })
})
```

---

## Mocks
Mocks are a type of mock that allows you to control the behavior of a function or object. They are useful for testing side effects, such as updating state or making API calls.

```javascript
// api.js
export async function fetchUser(userId) {
  const response = await fetch(`https://jsonplaceholder.typicode.com/users/${userId}`);
  if (!response.ok) {
    throw new Error('Network response was not ok');
  }
  return response.json();
}
```

```javascript
// api.test.js
import { describe, it, expect, vi, beforeEach } from 'vitest';
import { fetchUser } from './api';

describe('fetchUser', () => {
  beforeEach(() => {
    // Reset fetch mock before each test
    global.fetch = vi.fn();
  });

  it('should return user data when API call is successful', async () => {
    // Mock successful fetch response
    const mockUser = { id: 1, name: 'John Doe' };
    global.fetch.mockResolvedValue({
      ok: true,
      json: async () => mockUser,
    });

    const data = await fetchUser(1);
    expect(data).toEqual(mockUser);
    expect(global.fetch).toHaveBeenCalledWith('https://jsonplaceholder.typicode.com/users/1');
  });

  it('should throw an error if response is not ok', async () => {
    // Mock failed fetch response
    global.fetch.mockResolvedValue({
      ok: false,
    });

    await expect(fetchUser(1)).rejects.toThrow('Network response was not ok');
  });
});
```


## AAA (Arrange, Act, Assert) Principles
The AAA (Arrange, Act, Assert) principles are a set of best practices for writing unit tests in JavaScript. They help ensure that tests are clear, easy to read, and easy to maintain.

1. **Arrange**: This refers to setting up the necessary state for the test, such as creating test data, initializing variables, or setting up mocks.
2. **Act**: This refers to executing the code being tested, such as calling a function or making an API request.
3. **Assert**: This refers to verifying that the expected output matches the actual output.

