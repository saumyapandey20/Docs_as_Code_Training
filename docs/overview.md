# BMC AMI DevX Total Test overview

BMC AMI DevX Total Test is an automated testing solution for mainframe applications. It helps developers and testers create, execute, and maintain tests throughout the software development lifecycle.

By automating repeatable tests, development teams can validate code changes earlier, improve test coverage, and reduce the risk of introducing defects.

>[!Note]
>The available capabilities can vary depending on the installed product version, licensed components, and environment configuration.

## What can you do with BMC AMI DevX Total Test?

You can use BMC AMI DevX Total Test to:

- Test mainframe programs and subprograms.
- Create virtualized and non-virtualized tests.
- Perform unit, functional, integration, system, and regression testing.
- Reuse existing tests when applications change.
- Build regression test suites.
- incorporate automated testing into DevOps pipelines.
- Identify defects earlier in the development lifecycle.

## Key capabilities

| Capability | Description | Benefit |
|---|---|---|
| Automated testing | Automates the execution and validation of tests | Reduces repetitive manual testing |
| Virtualized testing | Tests program logic by simulating dependencies | Enables testing when dependent systems or data are unavailable |
| Non-virtualized testing | Tests programs by using actual resources and dependencies | Validates behavior in a connected environment |
| Regression testing | Repeats existing tests after code changes | Helps identify unintended changes |
| Pipeline integration | Incorporates testing into a DevOps pipeline | Supports continuous testing |
| Result reporting | Records test outcomes for analysis | Helps teams identify and investigate failures |

## Who uses BMC AMI DevX Total Test?

The following roles commonly use or benefit from the product:

- **Mainframe developers** create and run tests after changing application code.
- **Test engineers** build reusable tests and regression test suites.
- **DevOps engineers** incorporate automated tests into delivery pipelines.
- **Development leads** review test results and code-quality information.
- **Application owners** gain confidence that application changes have been validated.

## Types of testing

BMC AMI DevX Total Test supports testing at different stages of development.

| Test type | Purpose |
|---|---|
| Unit testing | Validates an individual program or unit of code |
| Functional testing | Confirms that a function produces the expected result |
| Integration testing | Validates interactions between application components |
| System testing | Tests application behavior in a broader system |
| Regression testing | Confirms that existing behavior continues after a change |

> **Important:** Select a testing approach that reflects the application, dependencies, test-data requirements, and risks associated with the code change.

## Virtualized and non-virtualized testing

### Virtualized testing

Virtualized testing isolates the program being tested from selected dependencies. Simulated inputs and expected outputs can be used to test program logic in a controlled environment.

Virtualized testing can be useful when:

- A dependent system is unavailable.
- Test data is difficult to create.
- Calling an external system is costly.
- A specific error condition must be reproduced.
- Testing must not affect a live system.

### Non-virtualized testing

Non-virtualized testing executes a program with its available resources and dependencies. It can help validate how the program behaves in a connected environment.

>[!Warning]
>A non-virtualized test might update data or call connected systems. Verify the test environment and test data before running the test.

## Typical testing workflow

The following workflow provides a simplified example:

1. Identify the program or component to test.
2. Define the test inputs and expected results.
3. Create a virtualized or non-virtualized test.
4. Execute the test.
5. Compare the actual and expected results.
6. Investigate and correct any failures.
7. Add the successful test to a regression test suite.
8. Run the test again when the application changes.

<img src="../image/Total Test.png" width="300" height="200" />

*Example of a development workflow that incorporates automated testing.*

## Example test definition

The following fictional YAML example demonstrates how a test definition might be represented. It is included only for Markdown practice and is not actual BMC AMI DevX Total Test syntax.

```yaml
test:
  name: Calculate customer discount
  program: CUSTDISC
  type: unit

input:
  customerType: GOLD
  orderAmount: 1000

expected:
  discountPercentage: 10
  finalAmount: 900
```

## DevOps pipeline integration

Automated tests can be incorporated into a DevOps pipeline so that code changes are tested consistently.

```text
Code change
    ↓
Build
    ↓
Execute automated tests
    ↓
Review test results
    ↓
Approve or reject the change
```

For example, a pipeline can stop a deployment when a required test fails.

```groovy
stage('Run mainframe tests') {
    steps {
        echo 'Execute the configured BMC AMI DevX Total Test suite'
    }
}
```

>[!Note]
>This pipeline code is illustrative. The actual configuration depends on your automation platform, plugins, product version, and environment.

## Benefits

Using automated testing can help an organization:

- Detect problems earlier.
- Increase test consistency and repeatability.
- Improve regression-test coverage.
- Reduce the effort required for repeated testing.
- Support faster and more frequent application changes.
- Provide test results that teams can review and track.

## Related products and solutions

BMC AMI DevX Total Test is part of the broader BMC AMI DevX portfolio. Depending on their environment, development teams might use it with other tools for:

- Source-code management
- Build and deployment automation
- Debugging and fault analysis
- Code-quality analysis
- Application visualization
- Continuous integration and continuous delivery

## Related topics

- [Getting started with BMC AMI DevX Total Test](getting-started.md)
- [Installing](installation.md)
- [Troubleshooting test failures](troubleshooting.md)

## Additional information

For current product information, see the [BMC AMI DevX Total Test product page](https://docs.bmc.com/xwiki/bin/view/Mainframe/DevX/BMC-AMI-DevX-Total-Test/badtt2601/).

## Next steps

After learning about BMC AMI DevX Total Test:

1. Confirm that your environment meets the product requirements.
2. Identify a program suitable for an initial test.
3. Determine whether the test should be virtualized or non-virtualized.
4. Create and execute your first test.
5. Review the results and add the test to a regression test suite.