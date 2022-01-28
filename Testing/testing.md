# Testing

1. Unit Testing - testing blocks of code (funcitons, modules, classes) -> write thousands of tests
2. Integration Testing - Dependencies/Modules are combined and tested together -> write a couple
3. E2E testing - Emulating a user -> write a few

We need 3 types of tools:

1. **Test runner** to execute tests (Mocha)
2. **Assertion Library** to define testing logic/conditions (Chai)
   - **Jest** is both a test runner and an assertion Library
3. Headless Browser (e2e) to simulate browser interaction (Puppeteer)
