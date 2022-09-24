# Testing thought
## Why we need test
### What is a good product to user
1. good experience
2. few bugs
3. quick fix on bugs

#### Example - Authentification
1. response quickly, correct authentification
2. less errors like encoding bug on password
3. if any bugs on encoding for example, it should be fixed quickly

### How test help to make product good
1. test can not make better user experience directly
2. but it can __help detect bugs and help developpers to fix bugs quickly.__ This should be the true value of tests.

## What is a good test
### Fast
Quick tests can give developpers quick feedbacks to failures, a quick feedback leads to a quick fix. Of course, no one wnat to wait hours to wait for the test result... A 100ms test can be considered as slow.

### Reliable
Tests should be reliable, if not the developper will lose trust in them. Like daily failed E2E tests, no one will look at them...

### Isolate failures
To fix a bug, the developpers need to find it first. On a large test which walks through thousands of lines of code, it is difficult to find the bugged line... On the other side, a small and well isolated test can locate the bug much easily.

## Unit test
Unit test is a basic type of test, which is:
1. fast and reliable, as it mocks external components (http service, db, local file systems...)
2. small, which only test one single unit

This type of test is the most eligible as a good test, we need to write more tests of this type. 

## Integration test
Unit test is not enough because there is always some interaction with external systems. It is necessary to test the integration. But this kind of test is often slower (e.g. http calls, db calls...).

## Functional test / E2E test
This type of test is on the perspective of user, it tests how a client use the product. So we do test by including all systems. But it also means the tests will be much slower and more instable.

## Testing pyramid
![test_pyramid](../resources/tests_pyramid.png)
number of tests: Unit > Integration > E2E

As a good first guess, Google often suggests a 70/20/10 split: 70% unit tests, 20% integration tests, and 10% end-to-end tests. The exact mix will be different for each team, but in general, it should retain that pyramid shape.

## Some test anti patterns
### Testing internal implementation
Donnot waste time to test interal implementation. Because the class can be refactored, tests on internal implementation can be broken by refactoring. It is not maintainable.
1. Test the busniess logic which is often exposed by the public methods.
2. Keep things simple. When writing tests, think always, "if I enter values x and y, will the result be z?" instead of "if I enter x and y, will the method call class A first, then call class B and then return the result of class A plus the result of class B?"

### Having flaky or slow tests
Explained above

### Treating test code as a second class citizen
Test code is also maintained by developpers, if the test code is hard to read and maintain, it is a pain to the developpers who debug and maintain them. Try to apply the principle coding philosophies to test codes like:
- DRY
- KISS
- SOLID

### Not converting production bugs to tests
A bug on production which cause impact to client is critical, the bugged code should also be considered as critical code. Critical code should be covered by tests.

## Divers
### Test units size
We can consider one class as a unit or several business related classes as a unit. To make the unit big or small, it all depends on developper.

For example: in a system, component A has 4 code paths, component B has 5 code paths, component C has 4 code paths.

If we consider A+B+C as a unit, how many code paths we have for this unit? 4*5*4=90. We need 90 unit tests to cover all the code paths.

What if we consider A, B, C as 3 units? 4+5+4=13 code paths in total for the 3 units. 13 unit tests to cover all the code paths.

Personnally, I will make unit small.

### Do not duplicate tests
Duplicate tests between unit test, integration test and E2E test has no extra value. If low level business logic is tested in unit test, there is no reason to add test to verify the the business logic again. The integration test should focus on the aspects which are not covered by unit test.

Two rules of thumb:
1. If a higher-level test spots an error and there's no lower-level test failing, you need to write a lower-level test
2. Push your tests as far down the test pyramid as you can 

### TDD ?
I tried before, but it was not my thing. I think it is a good pratice, but it is also the developper's choice. It is ok to follow it or not. Find the bug and protect the product is all to developpers and tests.

### Test automation
I think, idealy, for every save, at least the unit tests should be run. Every commit unit test and integration should be run. Every deploy, functional test should be run in SIT. All these need to be automated.

## References
1. [Just Say No to More End-to-End Tests](https://testing.googleblog.com/2015/04/just-say-no-to-more-end-to-end-tests.html)
2. [Software Testing Anti-patterns](https://blog.codepipes.com/testing/software-testing-antipatterns.html)
3. [The Practical Test Pyramid](https://martinfowler.com/articles/practical-test-pyramid.html)